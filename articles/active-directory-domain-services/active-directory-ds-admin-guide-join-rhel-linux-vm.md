---
title: "Az Azure Active Directory tartományi szolgáltatások: RHEL virtuális gép csatlakoztatása felügyelt tartományhoz |} Microsoft Docs"
description: "Red Hat Enterprise Linux virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások"
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
ms.openlocfilehash: 69f1850bfed90392e9a4695e2443ffaa6bfc746d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a><span data-ttu-id="96c38-103">Red Hat Enterprise Linux 7 virtuális gépek csatlakoztatása felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="96c38-103">Join a Red Hat Enterprise Linux 7 virtual machine to a managed domain</span></span>
<span data-ttu-id="96c38-104">Ez a cikk bemutatja, hogyan Red Hat Enterprise Linux (RHEL) 7 virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="96c38-104">This article shows you how to join a Red Hat Enterprise Linux (RHEL) 7 virtual machine to an Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="96c38-105">Red Hat Enterprise Linux virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="96c38-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="96c38-106">A következő lépésekkel rendszerű RHEL 7 virtuális gép az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="96c38-106">Perform the following steps to provision a RHEL 7 virtual machine using the Azure portal.</span></span>

1. <span data-ttu-id="96c38-107">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="96c38-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

    ![Azure-portál irányítópultjának](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="96c38-109">Kattintson a **új** a bal oldali ablaktáblán, és írja be **Red Hat** azokat a keresési sávon, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="96c38-109">Click **New** on the left pane and type **Red Hat** into the search bar as shown in the following screenshot.</span></span> <span data-ttu-id="96c38-110">A Red Hat Enterprise Linux jelennek meg a keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="96c38-110">Entries for Red Hat Enterprise Linux appear in the search results.</span></span> <span data-ttu-id="96c38-111">Kattintson a **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="96c38-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="96c38-113">A Keresés a **mindent** ablaktábla szerepelnie kell a Red Hat Enterprise Linux 7.2 kép.</span><span class="sxs-lookup"><span data-stu-id="96c38-113">The search results in the **Everything** pane should list the Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="96c38-114">Kattintson a **Red Hat Enterprise Linux 7.2** a virtuálisgép-lemezkép kapcsolatos további információk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="96c38-114">Click **Red Hat Enterprise Linux 7.2** to view more information about the virtual machine image.</span></span>

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="96c38-116">Az a **Red Hat Enterprise Linux 7.2** ablaktáblán, a virtuálisgép-lemezkép további információt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="96c38-116">In the **Red Hat Enterprise Linux 7.2** pane, you should see more information about the virtual machine image.</span></span> <span data-ttu-id="96c38-117">Az a **telepítési modell kiválasztása** legördülő menüben válasszon ki **klasszikus**.</span><span class="sxs-lookup"><span data-stu-id="96c38-117">In the **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="96c38-118">Kattintson a **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="96c38-118">Then click the **Create** button.</span></span>

    ![Kép részleteinek megtekintése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="96c38-120">A a **alapjai** oldalán a **hozzon létre virtuális gépet** varázslóban adja meg a **állomásnév** az új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="96c38-120">In the **Basics** page of the **Create virtual machine** wizard, enter the **Host Name** for the new virtual machine.</span></span> <span data-ttu-id="96c38-121">Is a helyi rendszergazdai felhasználónév megadása a **felhasználónév** mező és egy **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="96c38-121">Also specify a local administrator user name in the **User name** field and a **Password**.</span></span> <span data-ttu-id="96c38-122">Választhatja azt is, a helyi rendszergazda felhasználó hitelesítéséhez SSH-kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="96c38-122">You may also choose to use an SSH key to authenticate the local administrator user.</span></span> <span data-ttu-id="96c38-123">Jelölje ki a **Tarifacsomagot** a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="96c38-123">Also select a **Pricing Tier** for the virtual machine.</span></span>

    ![Hozzon létre virtuális Gépet - alapismeretek lap](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="96c38-125">Az a **mérete** oldalán a **hozzon létre virtuális gépet** varázsló, válassza ki a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="96c38-125">In the **Size** page of the **Create virtual machine** wizard, select the size for the virtual machine.</span></span>

    ![Hozzon létre Virtuálisgép - méret kiválasztása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="96c38-127">Az a **beállítások** oldalán a **hozzon létre virtuális gépet** lapján jelölje be a tárolási fiókot használja a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="96c38-127">In the **Settings** page of the **Create virtual machine** wizard, select the storage account for the virtual machine.</span></span> <span data-ttu-id="96c38-128">Kattintson a **virtuális hálózati** jelölje be a virtuális hálózatot, amelyhez a Linux virtuális gép kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="96c38-128">Click **Virtual Network** to select the virtual network to which the Linux VM should be deployed.</span></span> <span data-ttu-id="96c38-129">Az a **virtuális hálózati** panelen válassza az Azure AD tartományi szolgáltatásokat a virtuális hálózat érhető el.</span><span class="sxs-lookup"><span data-stu-id="96c38-129">In the **Virtual Network** blade, select the virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="96c38-130">Ebben a példában a "MyPreviewVNet" virtuális hálózati mentse azt.</span><span class="sxs-lookup"><span data-stu-id="96c38-130">In this example, we pick the 'MyPreviewVNet' virtual network.</span></span>

    ![Virtuális gép létrehozása – válassza ki a virtuális hálózat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="96c38-132">Az a **összegzés** oldalán a **hozzon létre virtuális gépet** varázsló, tekintse át, és kattintson a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="96c38-132">On the **Summary** page of the **Create virtual machine** wizard, review and click the **OK** button.</span></span>

    ![VM - kiválasztott virtuális hálózat létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="96c38-134">Központi telepítés az RHEL 7.2 lemezképen alapuló új virtuális gép elindul.</span><span class="sxs-lookup"><span data-stu-id="96c38-134">Deployment of the new virtual machine based on the RHEL 7.2 image should start.</span></span>

    ![Hozzon létre virtuális Gépet - KözpontiTelepítés elindítva](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="96c38-136">Néhány perc múlva a virtuális gép kell telepíteni sikeresen és készítenie való használatra.</span><span class="sxs-lookup"><span data-stu-id="96c38-136">After a few minutes, the virtual machine should be deployed successfully and ready for use.</span></span>

    ![Hozzon létre virtuális Gépet - telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="96c38-138">Távoli csatlakozás az újonnan kiépített Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="96c38-138">Connect remotely to the newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="96c38-139">Az RHEL 7.2 rendszerű virtuális gép az Azure-ban van kiépítve.</span><span class="sxs-lookup"><span data-stu-id="96c38-139">The RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="96c38-140">A következő feladathoz távolról csatlakozni a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="96c38-140">The next task is to connect remotely to the virtual machine.</span></span>

<span data-ttu-id="96c38-141">**Csatlakozás az RHEL 7.2 rendszerű virtuális géphez** kövesse a cikkben lévő utasítások [Linuxot futtató virtuális gép bejelentkezés](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="96c38-141">**Connect to the RHEL 7.2 virtual machine** Follow the instructions in the article [How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="96c38-142">A további lépések végrehajtása azt feltételezi, hogy a PuTTY SSH-ügyfél segítségével csatlakozzon az RHEL virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="96c38-142">The rest of the steps assume you use the PuTTY SSH client to connect to the RHEL virtual machine.</span></span> <span data-ttu-id="96c38-143">További információkért lásd: a [PuTTY letöltési oldalát](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="96c38-143">For more information, see the [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="96c38-144">A PuTTY programot nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="96c38-144">Open the PuTTY program.</span></span>
2. <span data-ttu-id="96c38-145">Adja meg a **állomásnév** az újonnan létrehozott RHEL virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="96c38-145">Enter the **Host Name** for the newly created RHEL virtual machine.</span></span> <span data-ttu-id="96c38-146">Ebben a példában a virtuális gép a gazdagép neve "contoso-rhel.cloudapp .net" van.</span><span class="sxs-lookup"><span data-stu-id="96c38-146">In this example, our virtual machine has the host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="96c38-147">Ha nem biztos a virtuális gép állomásnevét, tekintse meg a virtuális gép irányítópult az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="96c38-147">If you are not sure of the host name of your VM, refer to the VM dashboard on the Azure portal.</span></span>

    ![A puTTY-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="96c38-149">Jelentkezzen be a virtuális gépet a helyi rendszergazdai hitelesítő adatokat adja meg, hogy a virtuális gép létrehozásának használatával.</span><span class="sxs-lookup"><span data-stu-id="96c38-149">Log on to the virtual machine using the local administrator credentials you specified when the virtual machine was created.</span></span> <span data-ttu-id="96c38-150">Ebben a példában a helyi rendszergazdai fiók "mahesh" használtuk.</span><span class="sxs-lookup"><span data-stu-id="96c38-150">In this example, we used the local administrator account "mahesh".</span></span>

    ![PuTTY bejelentkezés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a><span data-ttu-id="96c38-152">A Linux virtuális gépre telepíti a szükséges csomagokat</span><span class="sxs-lookup"><span data-stu-id="96c38-152">Install required packages on the Linux virtual machine</span></span>
<span data-ttu-id="96c38-153">A virtuális géphez való csatlakozás után a következő feladathoz tartományhoz való csatlakozást, a virtuális gépen a szükséges csomagokat telepíteni fogja.</span><span class="sxs-lookup"><span data-stu-id="96c38-153">After connecting to the virtual machine, the next task is to install packages required for domain join on the virtual machine.</span></span> <span data-ttu-id="96c38-154">Hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="96c38-154">Perform the following steps:</span></span>

1. <span data-ttu-id="96c38-155">**Telepítse a realmd:** a realmd csomag szolgál a tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="96c38-155">**Install realmd:** The realmd package is used for domain join.</span></span> <span data-ttu-id="96c38-156">A PuTTY terminál írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="96c38-156">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="96c38-157">sudo yum telepítés realmd</span><span class="sxs-lookup"><span data-stu-id="96c38-157">sudo yum install realmd</span></span>

    ![Realmd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="96c38-159">Néhány perc múlva a realmd csomag telepítve kell beolvasni a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="96c38-159">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="96c38-161">**Telepítse a sssd:** a realmd csomag függ sssd tartomány összekapcsolási műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="96c38-161">**Install sssd:** The realmd package depends on sssd to perform domain join operations.</span></span> <span data-ttu-id="96c38-162">A PuTTY terminál írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="96c38-162">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="96c38-163">sudo yum telepítés sssd</span><span class="sxs-lookup"><span data-stu-id="96c38-163">sudo yum install sssd</span></span>

    ![Sssd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="96c38-165">Néhány perc múlva a sssd csomag telepítve kell beolvasni a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="96c38-165">After a few minutes, the sssd package should get installed on the virtual machine.</span></span>

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="96c38-167">**Telepítse a kerberos:** a PuTTY terminál, írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="96c38-167">**Install kerberos:** In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="96c38-168">sudo yum telepítés krb5-munkaállomás krb5-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="96c38-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Kerberos telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="96c38-170">Néhány perc múlva a realmd csomag telepítve kell beolvasni a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="96c38-170">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![A Kerberos telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a><span data-ttu-id="96c38-172">A Linux virtuális gép csatlakoztatása felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="96c38-172">Join the Linux virtual machine to the managed domain</span></span>
<span data-ttu-id="96c38-173">Most, hogy a szükséges csomagokat a Linux virtuális gépek vannak telepítve, a következő feladat a virtuális gép csatlakoztatása felügyelt tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="96c38-173">Now that the required packages are installed on the Linux virtual machine, the next task is to join the virtual machine to the managed domain.</span></span>

1. <span data-ttu-id="96c38-174">Az AAD tartományi szolgáltatásokra által kezelt tartomány felderítése.</span><span class="sxs-lookup"><span data-stu-id="96c38-174">Discover the AAD Domain Services managed domain.</span></span> <span data-ttu-id="96c38-175">A PuTTY terminál írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="96c38-175">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="96c38-176">sudo tartomány CONTOSO100.COM felderítése</span><span class="sxs-lookup"><span data-stu-id="96c38-176">sudo realm discover CONTOSO100.COM</span></span>

    ![A kezdőtartomány felderítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="96c38-178">Ha **a kezdőtartomány felderítése** nem tudja a kezelt tartományban található, győződjön meg arról, hogy a tartomány elérhető-e a virtuális gépről (próbálja ping).</span><span class="sxs-lookup"><span data-stu-id="96c38-178">If **realm discover** is unable to find your managed domain, ensure that the domain is reachable from the virtual machine (try ping).</span></span> <span data-ttu-id="96c38-179">Bizonyosodjon meg arról, hogy a virtuális gép valóban már alkalmazva van az azonos virtuális hálózatban, amelyben a felügyelt tartományra érhető el.</span><span class="sxs-lookup"><span data-stu-id="96c38-179">Also ensure that the virtual machine has indeed been deployed to the same virtual network in which the managed domain is available.</span></span>
2. <span data-ttu-id="96c38-180">Kerberos inicializálni.</span><span class="sxs-lookup"><span data-stu-id="96c38-180">Initialize kerberos.</span></span> <span data-ttu-id="96c38-181">A PuTTY terminál írja be a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="96c38-181">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="96c38-182">Adjon meg egy felhasználót, aki a "AAD DC rendszergazdák" csoportba tartozik.</span><span class="sxs-lookup"><span data-stu-id="96c38-182">Ensure that you specify a user who belongs to the 'AAD DC Administrators' group.</span></span> <span data-ttu-id="96c38-183">Csak ezek a felhasználók kapcsolódhatnak a számítógépek a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="96c38-183">Only these users can join computers to the managed domain.</span></span>

    <span data-ttu-id="96c38-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="96c38-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="96c38-186">Győződjön meg arról, hogy adja meg a tartomány nevét a nagybetűvel, más kinit sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="96c38-186">Ensure that you specify the domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="96c38-187">A számítógép csatlakoztatása a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="96c38-187">Join the machine to the domain.</span></span> <span data-ttu-id="96c38-188">A PuTTY terminál írja be a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="96c38-188">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="96c38-189">Adja meg az előző lépést ("kinit") megadott ugyanahhoz a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="96c38-189">Specify the same user you specified in the preceding step ('kinit').</span></span>

    <span data-ttu-id="96c38-190">sudo tartomány illesztési – részletes CONTOSO100.COM -U 'bob@CONTOSO100.COM"</span><span class="sxs-lookup"><span data-stu-id="96c38-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![A kezdőtartomány-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="96c38-192">Kell ("sikeresen regisztrált számítógép tartomány") üzenet jelenik meg, ha a gép sikeresen csatlakozott a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="96c38-192">You should get a message ("Successfully enrolled machine in realm") when the machine is successfully joined to the managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="96c38-193">Ellenőrizze a tartományhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="96c38-193">Verify domain join</span></span>
<span data-ttu-id="96c38-194">Gyorsan ellenőrizheti, hogy a gép sikeresen csatlakoztatva lett a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="96c38-194">You can quickly verify whether the machine has been successfully joined to the managed domain.</span></span> <span data-ttu-id="96c38-195">Csatlakozás az újonnan tartományhoz RHEL virtuális gép SSH és a tartományi felhasználói fiókot, és ellenőrizze a megoldódott a felhasználói fiók megfelelően.</span><span class="sxs-lookup"><span data-stu-id="96c38-195">Connect to the newly domain joined RHEL VM using SSH and a domain user account and then check to see if the user account is resolved correctly.</span></span>

1. <span data-ttu-id="96c38-196">A PuTTY terminálon, írja be a kapcsolódni a következő parancsot az újonnan tartományhoz RHEL virtuális gép SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="96c38-196">In your PuTTY terminal, type the following command to connect to the newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="96c38-197">A felügyelt tartományhoz tartozó tartományi fiókot használni (például "bob@CONTOSO100.COM" Ebben az esetben.)</span><span class="sxs-lookup"><span data-stu-id="96c38-197">Use a domain account that belongs to the managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="96c38-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="96c38-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="96c38-199">A PuTTY terminál írja be a következő parancsot, hogy ha a kezdőkönyvtár megfelelően inicializálva.</span><span class="sxs-lookup"><span data-stu-id="96c38-199">In your PuTTY terminal, type the following command to see if the home directory was initialized correctly.</span></span>

    <span data-ttu-id="96c38-200">pwd</span><span class="sxs-lookup"><span data-stu-id="96c38-200">pwd</span></span>
3. <span data-ttu-id="96c38-201">A PuTTY terminál írja be a következő parancsot, ha a csoporttagságot lehet megoldani az helyes megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="96c38-201">In your PuTTY terminal, type the following command to see if the group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="96c38-202">id</span><span class="sxs-lookup"><span data-stu-id="96c38-202">id</span></span>

<span data-ttu-id="96c38-203">Egy minta kimenet parancs a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="96c38-203">A sample output of these commands follows:</span></span>

![Ellenőrizze a tartományhoz való csatlakozást](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="96c38-205">Hibaelhárítás a tartományhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="96c38-205">Troubleshooting domain join</span></span>
<span data-ttu-id="96c38-206">Tekintse meg a [hibaelhárítás tartományhoz való csatlakozást](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) cikk.</span><span class="sxs-lookup"><span data-stu-id="96c38-206">Refer to the [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="96c38-207">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="96c38-207">Related Content</span></span>
* [<span data-ttu-id="96c38-208">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="96c38-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="96c38-209">Windows Server virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="96c38-209">Join a Windows Server virtual machine to an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="96c38-210">[Bejelentkezés a Linux rendszerű virtuális gép](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="96c38-210">[How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="96c38-211">Kerberos telepítése</span><span class="sxs-lookup"><span data-stu-id="96c38-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="96c38-212">Red Hat Enterprise Linux 7 - Windows-integrációs útmutatója</span><span class="sxs-lookup"><span data-stu-id="96c38-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
