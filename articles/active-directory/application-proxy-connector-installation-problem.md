---
title: "Az ügynök Proxy összekötőjének telepítése probléma |} Microsoft Docs"
description: "Az alkalmazásproxy-ügynök összekötő telepítésekor előfordulhat, hogy szembesülhetnek kapcsolatos problémák elhárítása"
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
ms.openlocfilehash: 91b3f6f3c8339647f568a509e9efd8e1fffb13dd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a><span data-ttu-id="54517-103">A probléma az ügynök Proxy összekötőjének telepítése</span><span class="sxs-lookup"><span data-stu-id="54517-103">Problem installing the Application Proxy Agent Connector</span></span>

<span data-ttu-id="54517-104">Microsoft AAD alkalmazásproxy-összekötő által használt kimenő kapcsolatokat és a belső tartomány közötti a felhő elérhető végpontot a kapcsolatot létesíteni egy belső tartomány-összetevő.</span><span class="sxs-lookup"><span data-stu-id="54517-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections to establish the connectivity from the cloud available endpoint to the internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="54517-105">Általános problémás területek Connector telepítése</span><span class="sxs-lookup"><span data-stu-id="54517-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="54517-106">Az összekötő telepítése nem sikerül, az alapvető ok esetén általában a következő területek közül:</span><span class="sxs-lookup"><span data-stu-id="54517-106">When the installation of a connector fails, the root cause is usually one of the following areas:</span></span>

1.  <span data-ttu-id="54517-107">**Kapcsolat** – sikeres telepítés, az új összekötő kell regisztrálni, és létrehozza a jövőbeli megbízhatósági tulajdonságok befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="54517-107">**Connectivity** – to complete a successful installation, the new connector needs to register and establish future trust properties.</span></span> <span data-ttu-id="54517-108">Ehhez a AAD alkalmazásproxy felhő szolgáltatáshoz való csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="54517-108">This is done by connecting to the AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="54517-109">**Megbízhatósági kapcsolat kialakítása** – az új összekötőt hoz létre egy önaláírt tanúsítványt, és regisztrálja a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="54517-109">**Trust Establishment** – the new connector creates a self-signed cert and registers to the cloud service.</span></span>

3.  <span data-ttu-id="54517-110">**A rendszergazda hitelesítési** – a telepítés során a felhasználónak meg kell adnia az összekötő telepítéséhez rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="54517-110">**Authentication of the admin** – during installation, the user must provide admin credentials to complete the Connector installation.</span></span>

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="54517-111">Ellenőrizze a kapcsolatot a Felhőbeli Proxy szolgáltatás és a Microsoft Login lap</span><span class="sxs-lookup"><span data-stu-id="54517-111">Verify connectivity to the Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="54517-112">**Cél:** ellenőrizze, hogy az összekötő számítógéphez csatlakozni tud-e az AAD alkalmazásproxy regisztrációs végpont, valamint a Microsoft bejelentkezési oldalt.</span><span class="sxs-lookup"><span data-stu-id="54517-112">**Objective:** Verify that the connector machine can connect to the AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="54517-113">Nyisson meg egy böngészőt, és nyissa meg a következő weblapra: <https://aadap-portcheck.connectorporttest.msappproxy.net> , és győződjön meg arról, hogy működik-e a csatlakozási porttal 9090 és 9091 központi Amerikai Egyesült Államok és USA keleti régiója adatközpontokhoz.</span><span class="sxs-lookup"><span data-stu-id="54517-113">Open a browser and go to the following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that the connectivity to Central US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="54517-114">Ha ezeket a portokat bármelyike nem sikeres (zöld pipa nem rendelkezik), ellenőrizze, hogy rendelkezik-e a tűzfal- vagy háttéradatbázis proxy \*. msappproxy.net 9090 és helyesen definiálva 9091 porttal.</span><span class="sxs-lookup"><span data-stu-id="54517-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that the Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="54517-115">Nyisson meg egy böngészőt (külön lapon), és nyissa meg a következő weblapra: <https://login.microsoftonline.com>, győződjön meg arról, hogy ez az oldal bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="54517-115">Open a browser (separate tab) and go to the following web page: <https://login.microsoftonline.com>, make sure that you can login to that page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="54517-116">Ellenőrizze a gép és a háttérkiszolgáló összetevői támogatják az alkalmazásproxy megbízható tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="54517-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="54517-117">**Cél:** győződjön meg arról, hogy az összekötő számítógéphez, a háttér-proxy és a tűzfal képesek támogatni a jövőbeli megbízhatóságához az összekötő által létrehozott tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="54517-117">**Objective:** Verify that the connector machine, backend proxy and firewall can support the certificate created by the connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="54517-118">Az összekötő megpróbál létrehozni egy SHA512 cert TLS1.2 által támogatott.</span><span class="sxs-lookup"><span data-stu-id="54517-118">The connector tries to create a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="54517-119">Ha a számítógép vagy a háttértűzfalat, proxy nem támogatja a TLS1.2, a telepítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="54517-119">If the machine or the backend firewall and proxy does not support TLS1.2, the installation fail.</span></span>
>
>

<span data-ttu-id="54517-120">**A probléma megoldása:**</span><span class="sxs-lookup"><span data-stu-id="54517-120">**To resolve the issue:**</span></span>

1.  <span data-ttu-id="54517-121">Ellenőrizze a gép támogatja-e TLS1.2 – 2012 R2 után minden Windows-verziók támogatnia kell a TLS 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="54517-121">Verify the machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="54517-122">Ha az összekötő gép 2012 R2 verzióját vagy előtt, győződjön meg arról, hogy a következő KB telepítve vannak-e a gépen: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="54517-122">If your connector machine is from a version of 2012 R2 or prior, make sure that the following KBs are installed on the machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="54517-123">A hálózati rendszergazdától, és kérje meg, hogy ellenőrizze, hogy a háttér-proxy és tűzfal nem blokkolja SHA512 a kimenő forgalom.</span><span class="sxs-lookup"><span data-stu-id="54517-123">Contact your network admin and ask to verify that the backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-to-install-the-connector"></a><span data-ttu-id="54517-124">Ellenőrizze, hogy az összekötő telepítéséhez használt felügyeleti</span><span class="sxs-lookup"><span data-stu-id="54517-124">Verify admin is used to install the connector</span></span>

<span data-ttu-id="54517-125">**Cél:** ellenőrizze, hogy a felhasználó megpróbálja telepíteni az összekötőt a rendszergazda a helyes hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="54517-125">**Objective:** Verify that the user who tries to install the connector is an administrator with correct credentials.</span></span> <span data-ttu-id="54517-126">A felhasználó jelenleg sikeres a telepítéshez egy globális rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="54517-126">Currently, the user must be a global administrator for the installation to succeed.</span></span>

<span data-ttu-id="54517-127">**Győződjön meg arról, hogy a hitelesítő adatok helyesek:**</span><span class="sxs-lookup"><span data-stu-id="54517-127">**To verify the credentials are correct:**</span></span>

<span data-ttu-id="54517-128">Csatlakozás <https://login.microsoftonline.com> és ugyanezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="54517-128">Connect to <https://login.microsoftonline.com> and use the same credentials.</span></span> <span data-ttu-id="54517-129">Győződjön meg arról, hogy a bejelentkezés sikeres.</span><span class="sxs-lookup"><span data-stu-id="54517-129">Make sure the login is successful.</span></span> <span data-ttu-id="54517-130">A felhasználói szerepkör megnyitásával ellenőrizheti **Azure Active Directory**  - &gt; **felhasználók és csoportok**  - &gt; **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="54517-130">You can check the user role by going to **Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="54517-131">Válassza ki a felhasználói fiókot, majd "Directory szerepkör" az eredményül kapott menüjében.</span><span class="sxs-lookup"><span data-stu-id="54517-131">Select your user account, then “Directory Role” in the resulting menu.</span></span> <span data-ttu-id="54517-132">Győződjön meg arról, hogy a kiválasztott "Globális rendszergazda".</span><span class="sxs-lookup"><span data-stu-id="54517-132">Verify that the selected role is “Global administrator”.</span></span> <span data-ttu-id="54517-133">Ha nem érhető el a oldalakon mentén ezeket a lépéseket, nem egy globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="54517-133">If you are unable to access any of the pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54517-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54517-134">Next steps</span></span>
[<span data-ttu-id="54517-135">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="54517-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
