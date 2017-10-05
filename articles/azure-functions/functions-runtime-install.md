---
title: "Azure Functions Runtime telepítésének |} Microsoft Docs"
description: "Az Azure Functions Futtatókörnyezettel telepítése"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 1e4188313a87d07f396e5f8edc8969dd5da2c436
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a><span data-ttu-id="b2ea7-103">Telepítse az Azure Functions futásidejű előnézete</span><span class="sxs-lookup"><span data-stu-id="b2ea7-103">Install the Azure Functions Runtime Preview</span></span>

<span data-ttu-id="b2ea7-104">Ha szeretné telepíteni az Azure Functions Futtatókörnyezettel előzetes, akkor kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b2ea7-104">If you would like to install the Azure Functions Runtime preview, you must follow these steps:</span></span>

1. <span data-ttu-id="b2ea7-105">Győződjön meg arról, a gép megfelel a minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="b2ea7-105">Ensure your machine passes the minimum requirements</span></span>
1. <span data-ttu-id="b2ea7-106">Töltse le a [Azure Functions futásidejű Preview telepítő](https://aka.ms/azafr).</span><span class="sxs-lookup"><span data-stu-id="b2ea7-106">Download the [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span></span> 
1. <span data-ttu-id="b2ea7-107">Telepítse az Azure Functions Futtatókörnyezettel előzetes</span><span class="sxs-lookup"><span data-stu-id="b2ea7-107">Install the Azure Functions Runtime preview</span></span>
1. <span data-ttu-id="b2ea7-108">Az Azure Functions Futtatókörnyezettel előzetes konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2ea7-108">Complete the configuration of the Azure Functions Runtime preview</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2ea7-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b2ea7-109">Prerequisites</span></span>

<span data-ttu-id="b2ea7-110">Az Azure Functions Futtatókörnyezettel előzetes verzió telepítése előtt az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="b2ea7-110">Before you install the Azure Functions Runtime preview, you must have the following:</span></span>

1. <span data-ttu-id="b2ea7-111">A Microsoft Windows Server 2016 vagy a Microsoft Windows 10 Creators frissítés (Professional és Enterprise Edition) rendszerű gépek.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-111">A machine running Microsoft Windows Server 2016 or Microsoft Windows 10 Creators Update (Professional or Enterprise Edition).</span></span>
1. <span data-ttu-id="b2ea7-112">A hálózaton belül futó SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-112">A SQL Server instance running within your network.</span></span>  <span data-ttu-id="b2ea7-113">Minimális edition követelmény az SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-113">Minimum edition requirement is SQL Server Express.</span></span>

## <a name="install-the-azure-functions-runtime-preview"></a><span data-ttu-id="b2ea7-114">Telepítse az Azure Functions futásidejű előnézete</span><span class="sxs-lookup"><span data-stu-id="b2ea7-114">Install the Azure Functions Runtime Preview</span></span>

<span data-ttu-id="b2ea7-115">Az Azure Functions Futtatókörnyezettel preview telepítő végigvezeti Önt az Azure Functions Futtatókörnyezettel előzetes felügyeleti és feldolgozói szerepkörök telepítését.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-115">The Azure Functions Runtime preview installer guides you through the installation of the Azure Functions Runtime preview Management and Worker Roles.</span></span>  <span data-ttu-id="b2ea7-116">Akkor lehet a felügyeleti és a feldolgozói szerepkör telepítése ugyanazon a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-116">It is possible to install the Management and Worker role on the same machine.</span></span>  <span data-ttu-id="b2ea7-117">Azonban további funkciók hozzáadása, telepítenie kell a további gépek alakzatot több Worker a functions méretezése tennie további feldolgozói szerepkörök is.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-117">However, as you add more Functions, you must deploy more worker roles on additional machines to be able to scale your functions onto multiple workers.</span></span>

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a><span data-ttu-id="b2ea7-118">A felügyeleti és a feldolgozói szerepkör telepítése ugyanazon a számítógépen</span><span class="sxs-lookup"><span data-stu-id="b2ea7-118">Install the Management and Worker Role on the same machine</span></span>

1. <span data-ttu-id="b2ea7-119">Futtassa az Azure Functions futásidejű Preview telepítőjét.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-119">Run the Azure Functions Runtime Preview Installer.</span></span>

    ![Az Azure Functions futásidejű Preview telepítő][1]

1. <span data-ttu-id="b2ea7-121">**Kattintson a Tovább gombra** előzetes túli az első szakasza a telepítő</span><span class="sxs-lookup"><span data-stu-id="b2ea7-121">**Click Next** advance past the first stage of the installer</span></span>
1. <span data-ttu-id="b2ea7-122">Ha mindenképpen olvassa el a használati a **EULA**, **jelölje be a jelölőnégyzetet** a feltételek elfogadásának és **kattintson a Tovább gombra** ahhoz, hogy.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-122">Once you have read the terms of the **EULA**, **check the box** to accept the terms and **click Next** to advance.</span></span>
1. <span data-ttu-id="b2ea7-123">Most válassza ki a szerepköröket, ezen a számítógépen telepítendő **funkciók szerepkör** és/vagy **funkciók feldolgozói szerepkör** és **kattintson a Tovább gombra**</span><span class="sxs-lookup"><span data-stu-id="b2ea7-123">Now select the roles you want to install on this machine **Functions Management Role** and/or **Functions Worker Role** and **Click Next**</span></span>

    ![Az Azure Functions futásidejű Preview Installer - szerepkör kiválasztása][3]

    > [!NOTE]
    > <span data-ttu-id="b2ea7-125">Telepítheti a **funkciók feldolgozói szerepkör** sok más számítógépeken ehhez, kövesse ezeket az utasításokat, és csak akkor válassza **funkciók feldolgozói szerepkör** a telepítőben.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-125">You can install the **Functions Worker Role** on many other machines to do so, follow these instructions, and only select **Functions Worker Role** in the installer.</span></span>

1. <span data-ttu-id="b2ea7-126">**Kattintson a Tovább gombra** kell rendelkeznie a **Azure Functions futásidejű telepítő** telepítése a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-126">**Click Next** to have the **Azure Functions Runtime Installer** install on your machine.</span></span>
1. <span data-ttu-id="b2ea7-127">Miután befejeződött a telepítő elindítja a **Azure Functions futásidejű konfigurációs eszköz**.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-127">Once complete the installer will launch the **Azure Functions Runtime Configuration tool**.</span></span>

    ![Az Azure Functions futásidejű Preview telepítő befejezése][5]

    > [!NOTE]
    > <span data-ttu-id="b2ea7-129">Ha telepíti az **Windows 10** és a **tároló** funkció nem korábban engedélyezve van, a **Azure Functions Futtatókörnyezettel** előfordulhat, hogy indítsa újra a számítógépet a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-129">If you are installing on **Windows 10** and the **Container** feature has not been previously enabled, the **Azure Functions Runtime** Installer prompts you to reboot your machine to complete the install.</span></span>

## <a name="configure-the-azure-functions-runtime"></a><span data-ttu-id="b2ea7-130">Az Azure Functions Futtatókörnyezettel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2ea7-130">Configure the Azure Functions Runtime</span></span>

<span data-ttu-id="b2ea7-131">Az Azure Functions Futtatókörnyezettel telepítéséhez a konfigurációt kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-131">To complete the Azure Functions Runtime installation you must complete the configuration.</span></span>

1. <span data-ttu-id="b2ea7-132">A **Azure Functions futásidejű konfigurációs eszköz** jeleníti meg, mely szerepkörök sincs telepítve a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-132">The **Azure Functions Runtime Configuration Tool** shows which roles are installed on your machine.</span></span>

    ![Az Azure Functions futásidejű Preview konfigurációs eszköz][6]

1. <span data-ttu-id="b2ea7-134">Kattintson a **adatbázis** lapra, adja meg a **kapcsolódási adatait. az SQL Server-példány** és **kattintson az alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-134">Click the **Database** tab, enter the **connection details for your SQL Server Instance** and **click Apply**.</span></span>  <span data-ttu-id="b2ea7-135">Ez adatbázis létrehozására az Azure Functions Futtatókörnyezettel sorrendben támogatásához szükséges a futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-135">This is required in order to the Azure Functions Runtime to create a database to support the Runtime.</span></span>
    
    ![Az Azure Functions futásidejű Preview adatbázis konfigurációja][7]

1. <span data-ttu-id="b2ea7-137">Kattintson a **hitelesítő adatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-137">Click the **Credentials** tab.</span></span>  <span data-ttu-id="b2ea7-138">Ezen a képernyőn létre kell hoznia két új hitelesítő adatokat a fájlmegosztás az Azure Functions üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-138">On this screen you must create two new credentials for use with a FileShare for hosting all your Azure Functions.</span></span>  <span data-ttu-id="b2ea7-139">**Adja meg a felhasználónevet és jelszót** kombinációk a **megosztás fájltulajdonos** és a **fájl megosztási felhasználói** kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-139">**Specify Username and Password** combinations for the **File Share Owner** and for the **File Share User** and click **Apply**.</span></span>

    ![Az Azure Functions futásidejű Preview hitelesítő adatok][8]

1. <span data-ttu-id="b2ea7-141">Kattintson a **fájlmegosztás** fülre.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-141">Click the **File Share** tab.</span></span>  <span data-ttu-id="b2ea7-142">Ez a képernyő meg kell adnia a részleteit a **fájlmegosztás helye**.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-142">In this screen you must specify the details of the **File Share location**.</span></span>  <span data-ttu-id="b2ea7-143">Ez az Ön hozható létre, vagy egy meglévő fájlmegosztást, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-143">This can be created for you or you can use an existing File Share and click **Apply**.</span></span>  <span data-ttu-id="b2ea7-144">Ha új fájlmegosztás helyét, adjon meg egy könyvtárat az Azure Functions futtatókörnyezete használja.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-144">If you select a new File Share location, you must specify a directory for use by the Azure Functions Runtime.</span></span>
    
    ![Az Azure Functions futásidejű Preview fájlmegosztás][9]

1. <span data-ttu-id="b2ea7-146">Kattintson a **IIS** fülre.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-146">Click the **IIS** tab.</span></span>  <span data-ttu-id="b2ea7-147">Ezen a lapon, amely az Azure funkciók Runtime telepítésének hozza létre az IIS a webhelyek részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-147">This tab shows the details of the websites in IIS that the Azure Functions Runtime Installation will create.</span></span>  <span data-ttu-id="b2ea7-148">**Kattintson az alkalmaz** befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-148">**Click Apply** to complete.</span></span>

    ![Az Azure Functions futásidejű előzetes IIS][10]

1. <span data-ttu-id="b2ea7-150">Kattintson a **szolgáltatások** fülre.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-150">Click the **Services** tab.</span></span>  <span data-ttu-id="b2ea7-151">Ezen a lapon, a szolgáltatások állapotát jeleníti meg az Azure Functions Futtatókörnyezettel telepítésében.</span><span class="sxs-lookup"><span data-stu-id="b2ea7-151">This tab shows the status of the services in your Azure Functions Runtime installation.</span></span>  <span data-ttu-id="b2ea7-152">Ha a kezdeti konfigurációt követően a **Azure Functions gazdaszolgáltatás aktiválási** nem fut kattintson **szolgáltatás indítása**</span><span class="sxs-lookup"><span data-stu-id="b2ea7-152">If after initial configuration the **Azure Functions Host Activation Service** is not running click **Start Service**</span></span>

    ![Az Azure Functions futásidejű Preview konfigurációs befejezése][11]

1. <span data-ttu-id="b2ea7-154">Végül keresse meg a **Azure Functions futásidejű portálon** ,`https://<machinename>/`</span><span class="sxs-lookup"><span data-stu-id="b2ea7-154">Finally browse to the **Azure Functions Runtime Portal** as `https://<machinename>/`</span></span>

    ![Az Azure Functions futásidejű a betekintő portálon][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png