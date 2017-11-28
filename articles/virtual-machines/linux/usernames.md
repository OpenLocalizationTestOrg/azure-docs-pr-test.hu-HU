---
title: "Kiválasztja a felhasználónevet Linux |} Microsoft Docs"
description: "Megtudhatja, hogyan válassza ki a felhasználói neveket Linux virtuális gép az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 1874d72e5f88816036667932371ff28704d186c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a><span data-ttu-id="b80d0-103">Felhasználónevek kiválasztása Azure-ban futtatott Linux esetén</span><span class="sxs-lookup"><span data-stu-id="b80d0-103">Selecting User Names for Linux on Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="b80d0-104">Amikor egy Linux virtuális gépet az Azure később használatával jelentkezzen be a virtuális gép nem legfelső szintű felhasználó nevét kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="b80d0-104">When you provision a Linux virtual machine on Azure you must specify the name of a non-root user that you can later use to log into the VM.</span></span> <span data-ttu-id="b80d0-105">Úgy is dönthet, az új felhasználó nevét, vagy ha a beállítás a klasszikus Azure portálon elfogadhatja az alapértelmezett neve "azureuser".</span><span class="sxs-lookup"><span data-stu-id="b80d0-105">You may choose the name of the new user, or if provisioning via the Azure classic portal you can accept the default name "azureuser".</span></span>

<span data-ttu-id="b80d0-106">A legtöbb esetben ez a felhasználó nem létezik az alapjául szolgáló lemezképhez, és létrehozza a telepítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="b80d0-106">In most cases this user won't exist on the base image and will be created during the provisioning process.</span></span> <span data-ttu-id="b80d0-107">Ha a felhasználó létezik a virtuális gép alapjául szolgáló lemezképhez, majd az Azure Linux ügynök egyszerűen állítja be a jelszót és/vagy SSH-kulcs az adott felhasználó, amikor a virtuális gép létrehozása a megadott adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b80d0-107">If the user exists on the base VM image, then the Azure Linux agent simply configures the password and/or SSH key for that user based on the information you specified when creating the VM.</span></span>

<span data-ttu-id="b80d0-108">**Azonban**, Linux, amely nem használható felhasználónevek álló készletet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="b80d0-108">**However**, Linux defines a set of user names that should not be used.</span></span> <span data-ttu-id="b80d0-109">Az üzembe helyezési folyamat fog **sikertelen** Ha kiépítéséhez Linux virtuális gép egy meglévő rendszer felhasználó, amelyet a 0 – 99 UID rendelkező felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="b80d0-109">The provisioning process will **fail** if you try to provision a Linux VM using an existing system user, which is defined as a user with UID 0-99.</span></span> <span data-ttu-id="b80d0-110">Tipikus példája a `root` felhasználó, amelynek UID 0.</span><span class="sxs-lookup"><span data-stu-id="b80d0-110">A typical example is the `root` user, which has UID 0.</span></span>

* <span data-ttu-id="b80d0-111">További információ: [Linux Standard Base - felhasználói azonosító tartományok](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span><span class="sxs-lookup"><span data-stu-id="b80d0-111">See also: [Linux Standard Base - User ID Ranges](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span></span>

<span data-ttu-id="b80d0-112">A CentOS és Ubuntu, hogy azt ne használja az Azure-on Linux virtuális gépek kiépítése közös beépített rendszer felhasználók listáját a következő:</span><span class="sxs-lookup"><span data-stu-id="b80d0-112">The following is a list of common built-in system users for CentOS and Ubuntu that you should avoid using when provisioning a Linux virtual machine on Azure.</span></span> <span data-ttu-id="b80d0-113">A lista létrehozási csak egy példa, olvassa el a dokumentációt a annak érdekében, hogy úgy dönt, a felhasználónév nem ütközik egy meglévő rendszer felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b80d0-113">This list is just an example, please refer to the documentation for your distribution to ensure that the username you choose does not conflict with an existing system user.</span></span>

## <a name="centos"></a><span data-ttu-id="b80d0-114">CentOS</span><span class="sxs-lookup"><span data-stu-id="b80d0-114">CentOS</span></span>
* <span data-ttu-id="b80d0-115">abrt</span><span class="sxs-lookup"><span data-stu-id="b80d0-115">abrt</span></span>
* <span data-ttu-id="b80d0-116">ADM</span><span class="sxs-lookup"><span data-stu-id="b80d0-116">adm</span></span>
* <span data-ttu-id="b80d0-117">hang</span><span class="sxs-lookup"><span data-stu-id="b80d0-117">audio</span></span>
* <span data-ttu-id="b80d0-118">bin</span><span class="sxs-lookup"><span data-stu-id="b80d0-118">bin</span></span>
* <span data-ttu-id="b80d0-119">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="b80d0-119">cdrom</span></span>
* <span data-ttu-id="b80d0-120">cgred</span><span class="sxs-lookup"><span data-stu-id="b80d0-120">cgred</span></span>
* <span data-ttu-id="b80d0-121">démon</span><span class="sxs-lookup"><span data-stu-id="b80d0-121">daemon</span></span>
* <span data-ttu-id="b80d0-122">dbus</span><span class="sxs-lookup"><span data-stu-id="b80d0-122">dbus</span></span>
* <span data-ttu-id="b80d0-123">kitárcsázáshoz</span><span class="sxs-lookup"><span data-stu-id="b80d0-123">dialout</span></span>
* <span data-ttu-id="b80d0-124">DIP</span><span class="sxs-lookup"><span data-stu-id="b80d0-124">dip</span></span>
* <span data-ttu-id="b80d0-125">lemez</span><span class="sxs-lookup"><span data-stu-id="b80d0-125">disk</span></span>
* <span data-ttu-id="b80d0-126">hajlékonylemez</span><span class="sxs-lookup"><span data-stu-id="b80d0-126">floppy</span></span>
* <span data-ttu-id="b80d0-127">FTP</span><span class="sxs-lookup"><span data-stu-id="b80d0-127">ftp</span></span>
* <span data-ttu-id="b80d0-128">FTP</span><span class="sxs-lookup"><span data-stu-id="b80d0-128">ftp</span></span>
* <span data-ttu-id="b80d0-129">játékok</span><span class="sxs-lookup"><span data-stu-id="b80d0-129">games</span></span>
* <span data-ttu-id="b80d0-130">Gopher</span><span class="sxs-lookup"><span data-stu-id="b80d0-130">gopher</span></span>
* <span data-ttu-id="b80d0-131">haldaemon</span><span class="sxs-lookup"><span data-stu-id="b80d0-131">haldaemon</span></span>
* <span data-ttu-id="b80d0-132">halt</span><span class="sxs-lookup"><span data-stu-id="b80d0-132">halt</span></span>
* <span data-ttu-id="b80d0-133">kmem</span><span class="sxs-lookup"><span data-stu-id="b80d0-133">kmem</span></span>
* <span data-ttu-id="b80d0-134">zárolás</span><span class="sxs-lookup"><span data-stu-id="b80d0-134">lock</span></span>
* <span data-ttu-id="b80d0-135">LP</span><span class="sxs-lookup"><span data-stu-id="b80d0-135">lp</span></span>
* <span data-ttu-id="b80d0-136">mail</span><span class="sxs-lookup"><span data-stu-id="b80d0-136">mail</span></span>
* <span data-ttu-id="b80d0-137">Man</span><span class="sxs-lookup"><span data-stu-id="b80d0-137">man</span></span>
* <span data-ttu-id="b80d0-138">Hiba</span><span class="sxs-lookup"><span data-stu-id="b80d0-138">mem</span></span>
* <span data-ttu-id="b80d0-139">nfsnobody</span><span class="sxs-lookup"><span data-stu-id="b80d0-139">nfsnobody</span></span>
* <span data-ttu-id="b80d0-140">senki sem</span><span class="sxs-lookup"><span data-stu-id="b80d0-140">nobody</span></span>
* <span data-ttu-id="b80d0-141">NTP</span><span class="sxs-lookup"><span data-stu-id="b80d0-141">ntp</span></span>
* <span data-ttu-id="b80d0-142">Operátor</span><span class="sxs-lookup"><span data-stu-id="b80d0-142">operator</span></span>
* <span data-ttu-id="b80d0-143">oprofile</span><span class="sxs-lookup"><span data-stu-id="b80d0-143">oprofile</span></span>
* <span data-ttu-id="b80d0-144">postdrop</span><span class="sxs-lookup"><span data-stu-id="b80d0-144">postdrop</span></span>
* <span data-ttu-id="b80d0-145">utótag</span><span class="sxs-lookup"><span data-stu-id="b80d0-145">postfix</span></span>
* <span data-ttu-id="b80d0-146">qpidd</span><span class="sxs-lookup"><span data-stu-id="b80d0-146">qpidd</span></span>
* <span data-ttu-id="b80d0-147">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="b80d0-147">root</span></span>
* <span data-ttu-id="b80d0-148">RPC</span><span class="sxs-lookup"><span data-stu-id="b80d0-148">rpc</span></span>
* <span data-ttu-id="b80d0-149">rpcuser</span><span class="sxs-lookup"><span data-stu-id="b80d0-149">rpcuser</span></span>
* <span data-ttu-id="b80d0-150">saslauth</span><span class="sxs-lookup"><span data-stu-id="b80d0-150">saslauth</span></span>
* <span data-ttu-id="b80d0-151">Leállítás</span><span class="sxs-lookup"><span data-stu-id="b80d0-151">shutdown</span></span>
* <span data-ttu-id="b80d0-152">slocate</span><span class="sxs-lookup"><span data-stu-id="b80d0-152">slocate</span></span>
* <span data-ttu-id="b80d0-153">sshd</span><span class="sxs-lookup"><span data-stu-id="b80d0-153">sshd</span></span>
* <span data-ttu-id="b80d0-154">stapdev</span><span class="sxs-lookup"><span data-stu-id="b80d0-154">stapdev</span></span>
* <span data-ttu-id="b80d0-155">stapusr</span><span class="sxs-lookup"><span data-stu-id="b80d0-155">stapusr</span></span>
* <span data-ttu-id="b80d0-156">Szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="b80d0-156">sync</span></span>
* <span data-ttu-id="b80d0-157">sys</span><span class="sxs-lookup"><span data-stu-id="b80d0-157">sys</span></span>
* <span data-ttu-id="b80d0-158">szalag</span><span class="sxs-lookup"><span data-stu-id="b80d0-158">tape</span></span>
* <span data-ttu-id="b80d0-159">Teszt</span><span class="sxs-lookup"><span data-stu-id="b80d0-159">test</span></span>
* <span data-ttu-id="b80d0-160">tcpdump parancsot</span><span class="sxs-lookup"><span data-stu-id="b80d0-160">tcpdump</span></span>
* <span data-ttu-id="b80d0-161">TTY</span><span class="sxs-lookup"><span data-stu-id="b80d0-161">tty</span></span>
* <span data-ttu-id="b80d0-162">felhasználók</span><span class="sxs-lookup"><span data-stu-id="b80d0-162">users</span></span>
* <span data-ttu-id="b80d0-163">utempter</span><span class="sxs-lookup"><span data-stu-id="b80d0-163">utempter</span></span>
* <span data-ttu-id="b80d0-164">utmp</span><span class="sxs-lookup"><span data-stu-id="b80d0-164">utmp</span></span>
* <span data-ttu-id="b80d0-165">uucp</span><span class="sxs-lookup"><span data-stu-id="b80d0-165">uucp</span></span>
* <span data-ttu-id="b80d0-166">vcsa</span><span class="sxs-lookup"><span data-stu-id="b80d0-166">vcsa</span></span>
* <span data-ttu-id="b80d0-167">Videó</span><span class="sxs-lookup"><span data-stu-id="b80d0-167">video</span></span>
* <span data-ttu-id="b80d0-168">kerék</span><span class="sxs-lookup"><span data-stu-id="b80d0-168">wheel</span></span>

## <a name="ubuntu"></a><span data-ttu-id="b80d0-169">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="b80d0-169">Ubuntu</span></span>
* <span data-ttu-id="b80d0-170">ADM</span><span class="sxs-lookup"><span data-stu-id="b80d0-170">adm</span></span>
* <span data-ttu-id="b80d0-171">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="b80d0-171">admin</span></span>
* <span data-ttu-id="b80d0-172">hang</span><span class="sxs-lookup"><span data-stu-id="b80d0-172">audio</span></span>
* <span data-ttu-id="b80d0-173">biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="b80d0-173">backup</span></span>
* <span data-ttu-id="b80d0-174">bin</span><span class="sxs-lookup"><span data-stu-id="b80d0-174">bin</span></span>
* <span data-ttu-id="b80d0-175">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="b80d0-175">cdrom</span></span>
* <span data-ttu-id="b80d0-176">crontab</span><span class="sxs-lookup"><span data-stu-id="b80d0-176">crontab</span></span>
* <span data-ttu-id="b80d0-177">démon</span><span class="sxs-lookup"><span data-stu-id="b80d0-177">daemon</span></span>
* <span data-ttu-id="b80d0-178">kitárcsázáshoz</span><span class="sxs-lookup"><span data-stu-id="b80d0-178">dialout</span></span>
* <span data-ttu-id="b80d0-179">DIP</span><span class="sxs-lookup"><span data-stu-id="b80d0-179">dip</span></span>
* <span data-ttu-id="b80d0-180">lemez</span><span class="sxs-lookup"><span data-stu-id="b80d0-180">disk</span></span>
* <span data-ttu-id="b80d0-181">Faxkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b80d0-181">fax</span></span>
* <span data-ttu-id="b80d0-182">hajlékonylemez</span><span class="sxs-lookup"><span data-stu-id="b80d0-182">floppy</span></span>
* <span data-ttu-id="b80d0-183">biztosító</span><span class="sxs-lookup"><span data-stu-id="b80d0-183">fuse</span></span>
* <span data-ttu-id="b80d0-184">játékok</span><span class="sxs-lookup"><span data-stu-id="b80d0-184">games</span></span>
* <span data-ttu-id="b80d0-185">gnats</span><span class="sxs-lookup"><span data-stu-id="b80d0-185">gnats</span></span>
* <span data-ttu-id="b80d0-186">IRC</span><span class="sxs-lookup"><span data-stu-id="b80d0-186">irc</span></span>
* <span data-ttu-id="b80d0-187">kmem</span><span class="sxs-lookup"><span data-stu-id="b80d0-187">kmem</span></span>
* <span data-ttu-id="b80d0-188">fekvő</span><span class="sxs-lookup"><span data-stu-id="b80d0-188">landscape</span></span>
* <span data-ttu-id="b80d0-189">libuuid</span><span class="sxs-lookup"><span data-stu-id="b80d0-189">libuuid</span></span>
* <span data-ttu-id="b80d0-190">lista</span><span class="sxs-lookup"><span data-stu-id="b80d0-190">list</span></span>
* <span data-ttu-id="b80d0-191">LP</span><span class="sxs-lookup"><span data-stu-id="b80d0-191">lp</span></span>
* <span data-ttu-id="b80d0-192">mail</span><span class="sxs-lookup"><span data-stu-id="b80d0-192">mail</span></span>
* <span data-ttu-id="b80d0-193">Man</span><span class="sxs-lookup"><span data-stu-id="b80d0-193">man</span></span>
* <span data-ttu-id="b80d0-194">MessageBus</span><span class="sxs-lookup"><span data-stu-id="b80d0-194">messagebus</span></span>
* <span data-ttu-id="b80d0-195">mlocate</span><span class="sxs-lookup"><span data-stu-id="b80d0-195">mlocate</span></span>
* <span data-ttu-id="b80d0-196">netdev</span><span class="sxs-lookup"><span data-stu-id="b80d0-196">netdev</span></span>
* <span data-ttu-id="b80d0-197">Hírek</span><span class="sxs-lookup"><span data-stu-id="b80d0-197">news</span></span>
* <span data-ttu-id="b80d0-198">senki sem</span><span class="sxs-lookup"><span data-stu-id="b80d0-198">nobody</span></span>
* <span data-ttu-id="b80d0-199">nogroup</span><span class="sxs-lookup"><span data-stu-id="b80d0-199">nogroup</span></span>
* <span data-ttu-id="b80d0-200">Operátor</span><span class="sxs-lookup"><span data-stu-id="b80d0-200">operator</span></span>
* <span data-ttu-id="b80d0-201">plugdev</span><span class="sxs-lookup"><span data-stu-id="b80d0-201">plugdev</span></span>
* <span data-ttu-id="b80d0-202">Proxy</span><span class="sxs-lookup"><span data-stu-id="b80d0-202">proxy</span></span>
* <span data-ttu-id="b80d0-203">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="b80d0-203">root</span></span>
* <span data-ttu-id="b80d0-204">SASL</span><span class="sxs-lookup"><span data-stu-id="b80d0-204">sasl</span></span>
* <span data-ttu-id="b80d0-205">árnyékmásolat</span><span class="sxs-lookup"><span data-stu-id="b80d0-205">shadow</span></span>
* <span data-ttu-id="b80d0-206">src</span><span class="sxs-lookup"><span data-stu-id="b80d0-206">src</span></span>
* <span data-ttu-id="b80d0-207">ssh</span><span class="sxs-lookup"><span data-stu-id="b80d0-207">ssh</span></span>
* <span data-ttu-id="b80d0-208">sshd</span><span class="sxs-lookup"><span data-stu-id="b80d0-208">sshd</span></span>
* <span data-ttu-id="b80d0-209">személyzet</span><span class="sxs-lookup"><span data-stu-id="b80d0-209">staff</span></span>
* <span data-ttu-id="b80d0-210">sudo</span><span class="sxs-lookup"><span data-stu-id="b80d0-210">sudo</span></span>
* <span data-ttu-id="b80d0-211">Szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="b80d0-211">sync</span></span>
* <span data-ttu-id="b80d0-212">sys</span><span class="sxs-lookup"><span data-stu-id="b80d0-212">sys</span></span>
* <span data-ttu-id="b80d0-213">syslog</span><span class="sxs-lookup"><span data-stu-id="b80d0-213">syslog</span></span>
* <span data-ttu-id="b80d0-214">szalag</span><span class="sxs-lookup"><span data-stu-id="b80d0-214">tape</span></span>
* <span data-ttu-id="b80d0-215">TTY</span><span class="sxs-lookup"><span data-stu-id="b80d0-215">tty</span></span>
* <span data-ttu-id="b80d0-216">felhasználók</span><span class="sxs-lookup"><span data-stu-id="b80d0-216">users</span></span>
* <span data-ttu-id="b80d0-217">utmp</span><span class="sxs-lookup"><span data-stu-id="b80d0-217">utmp</span></span>
* <span data-ttu-id="b80d0-218">uucp</span><span class="sxs-lookup"><span data-stu-id="b80d0-218">uucp</span></span>
* <span data-ttu-id="b80d0-219">Videó</span><span class="sxs-lookup"><span data-stu-id="b80d0-219">video</span></span>
* <span data-ttu-id="b80d0-220">hangalámondás</span><span class="sxs-lookup"><span data-stu-id="b80d0-220">voice</span></span>
* <span data-ttu-id="b80d0-221">whoopsie</span><span class="sxs-lookup"><span data-stu-id="b80d0-221">whoopsie</span></span>
* <span data-ttu-id="b80d0-222">www-adatok</span><span class="sxs-lookup"><span data-stu-id="b80d0-222">www-data</span></span>

