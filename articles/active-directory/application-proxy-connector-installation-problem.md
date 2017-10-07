---
title: "Alkalmazásproxy-összekötő ügynök aaaProblem telepítése hello |} Microsoft Docs"
description: "Hogyan tootroubleshoot kapcsolatban, hogy előfordulhat, hogy telepítésekor arcfelismerési hello alkalmazásproxy-összekötő ügynök"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a><span data-ttu-id="bd322-103">A probléma hello ügynök Proxy összekötőjének telepítése</span><span class="sxs-lookup"><span data-stu-id="bd322-103">Problem installing hello Application Proxy Agent Connector</span></span>

<span data-ttu-id="bd322-104">Microsoft AAD alkalmazásproxy-összekötő egy belső tartomány-összetevő által használt kimenő kapcsolatok tooestablish hello kapcsolat hello felhő elérhető végpontot toohello belső tartomány.</span><span class="sxs-lookup"><span data-stu-id="bd322-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections tooestablish hello connectivity from hello cloud available endpoint toohello internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="bd322-105">Általános problémás területek Connector telepítése</span><span class="sxs-lookup"><span data-stu-id="bd322-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="bd322-106">Ha egy összekötő hello telepítése nem sikerül, hello okozza-e általában hello a következő területek közül:</span><span class="sxs-lookup"><span data-stu-id="bd322-106">When hello installation of a connector fails, hello root cause is usually one of hello following areas:</span></span>

1.  <span data-ttu-id="bd322-107">**Kapcsolat** – toocomplete a sikeres telepítés hello új összekötő igények tooregister, és létrehozza a jövőbeli megbízhatósági tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="bd322-107">**Connectivity** – toocomplete a successful installation, hello new connector needs tooregister and establish future trust properties.</span></span> <span data-ttu-id="bd322-108">Csatlakozás toohello AAD alkalmazásproxy felhőalapú szolgáltatás végzi.</span><span class="sxs-lookup"><span data-stu-id="bd322-108">This is done by connecting toohello AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="bd322-109">**Megbízhatósági kapcsolat kialakítása** – hello új összekötőt hoz létre egy önaláírt tanúsítványt, és regisztrálja a toohello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bd322-109">**Trust Establishment** – hello new connector creates a self-signed cert and registers toohello cloud service.</span></span>

3.  <span data-ttu-id="bd322-110">**Üdvözöljük a rendszergazdákat a hitelesítési** – a telepítés során hello felhasználónak meg kell adnia rendszergazdai hitelesítő adataival toocomplete hello összekötő telepítése.</span><span class="sxs-lookup"><span data-stu-id="bd322-110">**Authentication of hello admin** – during installation, hello user must provide admin credentials toocomplete hello Connector installation.</span></span>

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="bd322-111">Ellenőrizze a kapcsolat toohello Felhőbeli Proxy szolgáltatás és a Microsoft Login lap</span><span class="sxs-lookup"><span data-stu-id="bd322-111">Verify connectivity toohello Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="bd322-112">**Cél:** győződjön meg arról, hogy hello összekötő gép is kapcsolódnak toohello AAD alkalmazásproxy regisztrációs végpont, valamint a Microsoft bejelentkezési oldalt.</span><span class="sxs-lookup"><span data-stu-id="bd322-112">**Objective:** Verify that hello connector machine can connect toohello AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="bd322-113">Nyisson meg egy böngészőt, majd a toohello, a következő weblapot: <https://aadap-portcheck.connectorporttest.msappproxy.net> , és győződjön meg arról, hogy hello kapcsolat tooCentral USA és az USA keleti régiója adatközpontok porttal 9090 és 9091 megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="bd322-113">Open a browser and go toohello following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that hello connectivity tooCentral US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="bd322-114">Ha ezeket a portokat bármelyike nem sikeres (zöld pipa nem rendelkezik), győződjön meg arról, hogy hello tűzfal vagy a háttér-proxyra \*. msappproxy.net 9090 és helyesen definiálva 9091 porttal.</span><span class="sxs-lookup"><span data-stu-id="bd322-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that hello Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="bd322-115">Nyisson meg egy böngészőt (külön lapon), és nyissa meg a következő weblapot toohello: <https://login.microsoftonline.com>, győződjön meg arról, hogy toothat lap bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="bd322-115">Open a browser (separate tab) and go toohello following web page: <https://login.microsoftonline.com>, make sure that you can login toothat page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="bd322-116">Ellenőrizze a gép és a háttérkiszolgáló összetevői támogatják az alkalmazásproxy megbízható tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="bd322-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="bd322-117">**Cél:** ellenőrizze hello összekötő számítógéphez, a háttér-proxy és a tűzfal jövőbeli megbízhatósági hello-összekötő által létrehozott hello tanúsítvány is támogatja.</span><span class="sxs-lookup"><span data-stu-id="bd322-117">**Objective:** Verify that hello connector machine, backend proxy and firewall can support hello certificate created by hello connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="bd322-118">hello összekötő megkísérli toocreate TLS1.2 által támogatott SHA512 tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bd322-118">hello connector tries toocreate a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="bd322-119">Ha hello gép vagy a hello háttértűzfalat és a proxy nem támogatja a TLS1.2, hello telepítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="bd322-119">If hello machine or hello backend firewall and proxy does not support TLS1.2, hello installation fail.</span></span>
>
>

<span data-ttu-id="bd322-120">**tooresolve hello problémát:**</span><span class="sxs-lookup"><span data-stu-id="bd322-120">**tooresolve hello issue:**</span></span>

1.  <span data-ttu-id="bd322-121">Ellenőrizze a gép hello támogatja-e TLS1.2 – 2012 R2 után minden Windows-verziók támogatnia kell a TLS 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="bd322-121">Verify hello machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="bd322-122">Ha az összekötő gép 2012 R2 verzióját vagy előtt, győződjön meg arról, hogy hello hello számítógépen telepítve vannak a következő KB: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="bd322-122">If your connector machine is from a version of 2012 R2 or prior, make sure that hello following KBs are installed on hello machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="bd322-123">A hálózati rendszergazdától, és tooverify kérje meg, hogy hello háttér proxy és tűzfal nem blokkolja SHA512 a kimenő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="bd322-123">Contact your network admin and ask tooverify that hello backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a><span data-ttu-id="bd322-124">Ellenőrizze a használt tooinstall hello összekötő feladata</span><span class="sxs-lookup"><span data-stu-id="bd322-124">Verify admin is used tooinstall hello connector</span></span>

<span data-ttu-id="bd322-125">**Cél:** győződjön meg arról, hogy hello felhasználó megpróbál tooinstall hello összekötő rendszergazda helyes hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="bd322-125">**Objective:** Verify that hello user who tries tooinstall hello connector is an administrator with correct credentials.</span></span> <span data-ttu-id="bd322-126">Jelenleg hello felhasználói hello telepítési toosucceed egy globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd322-126">Currently, hello user must be a global administrator for hello installation toosucceed.</span></span>

<span data-ttu-id="bd322-127">**tooverify hello hitelesítő adatai helyesek:**</span><span class="sxs-lookup"><span data-stu-id="bd322-127">**tooverify hello credentials are correct:**</span></span>

<span data-ttu-id="bd322-128">Csatlakozás túl<https://login.microsoftonline.com> és hello ugyanazokat a hitelesítő adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="bd322-128">Connect too<https://login.microsoftonline.com> and use hello same credentials.</span></span> <span data-ttu-id="bd322-129">Ellenőrizze, hogy hello bejelentkezési sikeres.</span><span class="sxs-lookup"><span data-stu-id="bd322-129">Make sure hello login is successful.</span></span> <span data-ttu-id="bd322-130">Hello felhasználói szerepkör túl megnyitásával ellenőrizheti**Azure Active Directory**  - &gt; **felhasználók és csoportok**  - &gt; **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bd322-130">You can check hello user role by going too**Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="bd322-131">Válassza ki a felhasználói fiókot, majd "Directory szerepkör" hello eredményül kapott menüben.</span><span class="sxs-lookup"><span data-stu-id="bd322-131">Select your user account, then “Directory Role” in hello resulting menu.</span></span> <span data-ttu-id="bd322-132">Győződjön meg arról, hogy hello kijelölt szerepkör "Globális rendszergazda".</span><span class="sxs-lookup"><span data-stu-id="bd322-132">Verify that hello selected role is “Global administrator”.</span></span> <span data-ttu-id="bd322-133">Ha bármelyik hello lépések mentén lapok nem tooaccess, nem egy globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bd322-133">If you are unable tooaccess any of hello pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd322-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd322-134">Next steps</span></span>
[<span data-ttu-id="bd322-135">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="bd322-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
