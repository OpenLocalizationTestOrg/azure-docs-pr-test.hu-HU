---
title: "RemoteApp hibrid gyűjtemények létrehozásáról a aaaTroubleshoot |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot RemoteApp hibrid gyűjtemény létrehozása sikertelen"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a><span data-ttu-id="49597-103">Hibaelhárítás Azure RemoteApp hibrid gyűjtemények létrehozásával</span><span class="sxs-lookup"><span data-stu-id="49597-103">Troubleshoot creating Azure RemoteApp hybrid collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="49597-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="49597-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="49597-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="49597-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="49597-106">Hibrid gyűjtemény üzemel, és tárolja az hello Azure felhőben, de is megadható, hogy felhasználók hozzáférést adatokhoz és erőforrásokhoz, a helyi hálózaton tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="49597-106">A hybrid collection is hosted in and stores data in hello Azure cloud but also lets users access data and resources stored on your local network.</span></span> <span data-ttu-id="49597-107">A felhasználók az Azure Active Directoryval szinkronizált vagy összevont vállalati hitelesítő adataikkal jelentkezhetnek be, és érhetik el az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="49597-107">Users can access apps by logging in with their corporate credentials synchronized or federated with Azure Active Directory.</span></span> <span data-ttu-id="49597-108">Telepíthet egy meglévő Azure virtuális hálózat használó hibrid gyűjteményt, vagy létrehozhat egy új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="49597-108">You can deploy a hybrid collection that uses an existing Azure Virtual Network, or you can create a new virtual network.</span></span> <span data-ttu-id="49597-109">Azt javasoljuk, hogy hozzon létre, vagy használható egy virtuális hálózati alhálózat CIDR-tartomány elég nagy a várt jövőbeni növekedés Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="49597-109">We recommend that you create or use a virtual network subnet with a CIDR range large enough for expected future growth for Azure RemoteApp.</span></span>

<span data-ttu-id="49597-110">A gyűjtemény még nem hozott létre?</span><span class="sxs-lookup"><span data-stu-id="49597-110">Haven't created your collection yet?</span></span> <span data-ttu-id="49597-111">Lásd: [hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md) hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="49597-111">See [Create a hybrid collection](remoteapp-create-hybrid-deployment.md) for hello steps.</span></span>

<span data-ttu-id="49597-112">Ha problémája van a gyűjtemény létrehozása, vagy ha hello gyűjtemény nem működik hello módon úgy gondolja, hogy kell-e, tekintse meg a következő információk hello.</span><span class="sxs-lookup"><span data-stu-id="49597-112">If you are having trouble creating your collection, or if hello collection isn't working hello way you think it should, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="49597-113">A kép érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="49597-113">Your image is invalid</span></span>
<span data-ttu-id="49597-114">Ha megjelenik egy üzenet, például "GoldImageInvalid", ha vár a Azure tooprovision a gyűjtemény, az azt jelenti, hogy a sablon lemezképe nem felel meg hello [kép követelmények definiált](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="49597-114">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="49597-115">Igen, nyissa meg olvasni azokat [követelmények](remoteapp-imagereqs.md), javítsa ki a lemezkép és toocreate próbálkozzon újra a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="49597-115">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="does-your-vnet-have-network-security-groups-defined"></a><span data-ttu-id="49597-116">Rendelkezik a virtuális hálózat hálózati biztonsági csoportok definiált?</span><span class="sxs-lookup"><span data-stu-id="49597-116">Does your VNET have network security groups defined?</span></span>
<span data-ttu-id="49597-117">Hálózati biztonsági csoportokat használ a gyűjtemény hello alhálózaton definiált, győződjön meg arról, hogy ezek [URL-címek és portok](remoteapp-ports.md) érhető el az alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="49597-117">If you have network security groups defined on hello subnet you are using for your collection, make sure these [URLs and ports](remoteapp-ports.md) are accessible from within your subnet.</span></span>

<span data-ttu-id="49597-118">További hálózati biztonsági csoportok toohello telepített virtuális gépek által hello ad-alhálózatot adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="49597-118">You can add additional network security groups toohello VMs deployed by you in hello subnet for tighter control.</span></span>

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a><span data-ttu-id="49597-119">A saját DNS-kiszolgálók használata?</span><span class="sxs-lookup"><span data-stu-id="49597-119">Are you using your own DNS servers?</span></span> <span data-ttu-id="49597-120">És azok elérhető a virtuális hálózat alhálózatból?</span><span class="sxs-lookup"><span data-stu-id="49597-120">And are they accessible from your VNET subnet?</span></span>
> [!NOTE]
> <span data-ttu-id="49597-121">Toomake meg arról, hogy hello a virtuális hálózat DNS-kiszolgálók mindig be, és mindig tudja tooresolve hello virtuális géphez a virtuális hálózat hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="49597-121">You have toomake sure hello DNS servers in your VNET are always up and always able tooresolve hello virtual machines hosted in hello VNET.</span></span> <span data-ttu-id="49597-122">Ez a Google DNS ne használjon.</span><span class="sxs-lookup"><span data-stu-id="49597-122">Don't use Google DNS for this.</span></span>
> 
> 

<span data-ttu-id="49597-123">A hibrid gyűjtemények használhatja a saját DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="49597-123">For hybrid collections you use your own DNS servers.</span></span> <span data-ttu-id="49597-124">Azt adja meg azokat a hálózati konfigurációs séma vagy hello felügyeleti portálon keresztül a virtuális hálózat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="49597-124">You specify them in your network configuration schema or through hello management portal when you create your virtual network.</span></span> <span data-ttu-id="49597-125">DNS-kiszolgálók hello ahhoz, hogy azok van megadva (megakadályozását tooround multiplexelés) feladatátvételi módon használt.</span><span class="sxs-lookup"><span data-stu-id="49597-125">DNS servers are used in hello order that they are specified in a failover manner (as opposed tooround robin).</span></span>  
<span data-ttu-id="49597-126">Tekintse meg a túl[névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) arról, hogy a DNS-kiszolgálók konfigurált correcly toomake.</span><span class="sxs-lookup"><span data-stu-id="49597-126">Please refer too[Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake sure your DNS servers are configured correcly.</span></span>

<span data-ttu-id="49597-127">Győződjön meg arról a gyűjtemény hello DNS-kiszolgáló elérhető és érhetők el az ebben a gyűjteményben megadott hello VNET alhálózati.</span><span class="sxs-lookup"><span data-stu-id="49597-127">Make sure hello DNS servers for your collection are accessible and available from hello VNET subnet you specified for this collection.</span></span>

<span data-ttu-id="49597-128">Példa:</span><span class="sxs-lookup"><span data-stu-id="49597-128">For example:</span></span>

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![A DNS-kiszolgáló megadása](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a><span data-ttu-id="49597-130">Használja az Active Directory-tartományvezérlő a gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="49597-130">Are you using an Active Directory domain controller in your collection?</span></span>
<span data-ttu-id="49597-131">Jelenleg csak az Active Directory-tartomány egy Azure RemoteApp társítva.</span><span class="sxs-lookup"><span data-stu-id="49597-131">Currently only one Active Directory domain can be associated with Azure RemoteApp.</span></span> <span data-ttu-id="49597-132">hello hibrid gyűjtemény csak Azure Active Directory-fiókok egy Windows Server Active Directory-telepítésből; DirSync eszközzel szinkronizált támogat pontosabban vagy szinkronizálása megtörtént-e a jelszó-szinkronizálási lehetőséggel hello szinkronizálása megtörtént-e az Active Directory összevonási szolgáltatások (AD FS) összevonásának konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="49597-132">hello hybrid collection supports only Azure Active Directory accounts that have been synced using DirSync tool from a Windows Server Active Directory deployment; specifically, either synced with hello Password Synchronization option or synced with Active Directory Federation Services (AD FS) federation configured.</span></span> <span data-ttu-id="49597-133">Egyéni tartományt, amely megfelel a helyi tartomány hello UPN-tartomány utótag toocreate kell, és a címtár-integráció beállítása.</span><span class="sxs-lookup"><span data-stu-id="49597-133">You need toocreate a custom domain that matches hello UPN domain suffix for your on-premises domain and set up directory integration.</span></span>

<span data-ttu-id="49597-134">Lásd: [Active Directory konfigurálása az Azure RemoteApp](remoteapp-ad.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="49597-134">See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for more information.</span></span>

<span data-ttu-id="49597-135">Ellenőrizze, hogy megadott hello tartomány adatok érvényesek, és hello tartományvezérlő hello Azure távoli App használt hello alhálózati létrehozott virtuális gép elérhető.</span><span class="sxs-lookup"><span data-stu-id="49597-135">Make sure hello domain details provided are valid and hello domain controller is reachable from hello VM created in hello subnet used for Azure Remote App.</span></span> <span data-ttu-id="49597-136">Győződjön meg arról is hello szolgáltatás fiók a megadott hitelesítő adatok megadták az engedélyek tooadd számítógépek toohello tartományhoz, és hogy hello AD neve megadott hello DNS hello virtuális hálózat szerepel a feloldhatók legyenek.</span><span class="sxs-lookup"><span data-stu-id="49597-136">Also make sure hello service account credentials supplied have permissions tooadd computers toohello provided domain and that hello AD name provided can be resolved from hello DNS provided in hello VNET.</span></span>

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a><span data-ttu-id="49597-137">Milyen tartománynév adta meg a gyűjtemény létrehozásakor?</span><span class="sxs-lookup"><span data-stu-id="49597-137">What domain name did you specify when you created your collection?</span></span>
<span data-ttu-id="49597-138">hello tartománynév létrehozott vagy fel lehet egy belső tartománynevet (nem az Azure AD tartományi neve), és feloldható DNS-formátumban (contoso.local) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="49597-138">hello domain name you created or added must be an internal domain name (not your Azure AD domain name) and must be in resolvable DNS format (contoso.local).</span></span> <span data-ttu-id="49597-139">Például van egy Active Directory belső nevét (contoso.local), és az Active Directory egyszerű felhasználónév (contoso.com) - rendelkezik toouse hello belső nevét a gyűjtemény létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="49597-139">For example, you have an Active Directory internal name (contoso.local) and an Active Directory UPN (contoso.com) - you have toouse hello internal name when you create your collection.</span></span>

