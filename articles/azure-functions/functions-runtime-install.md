---
title: "aaaAzure funkciók Runtime telepítésének |} Microsoft Docs"
description: "Hogyan tooInstall hello Azure Functions Futtatókörnyezettel"
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
ms.openlocfilehash: 67c6d10b5c0ac43e880d29cff0ae7b099f82bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-functions-runtime-preview"></a>Hello Azure Functions futásidejű Preview telepítése

Ha szeretné tooinstall hello Azure Functions Futtatókörnyezettel preview, akkor kövesse az alábbi lépéseket:

1. Győződjön meg arról, a gép megfelelt hello minimális követelményeknek
1. Töltse le a hello [Azure Functions futásidejű Preview telepítő](https://aka.ms/azafr). 
1. Hello Azure Functions Futtatókörnyezettel preview telepítése
1. Hello Azure Functions Futtatókörnyezettel preview teljes hello konfigurálása

## <a name="prerequisites"></a>Előfeltételek

Hello Azure Functions Futtatókörnyezettel előzetes verzió telepítése előtt rendelkeznie kell hello következő:

1. A Microsoft Windows Server 2016 vagy a Microsoft Windows 10 Creators frissítés (Professional és Enterprise Edition) rendszerű gépek.
1. A hálózaton belül futó SQL Server-példányt.  Minimális edition követelmény az SQL Server Express.

## <a name="install-hello-azure-functions-runtime-preview"></a>Hello Azure Functions futásidejű Preview telepítése

hello Azure Functions Futtatókörnyezettel preview telepítő végigvezeti hello Azure Functions Futtatókörnyezettel előzetes felügyeleti és feldolgozói szerepkörök hello telepítését.  Lehetséges tooinstall hello felügyeleti és a feldolgozói szerepkör a hello egyazon számítógépen.  Azonban további funkciók hozzáadása, telepítenie kell a további gépek toobe képes tooscale további feldolgozói szerepkörök a funkciók több Worker-kiszolgálóra.

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a>Hello felügyeleti és a feldolgozói szerepkör telepítését hello egyazon számítógépen

1. Hello Azure Functions futásidejű Preview telepítő futtatásához.

    ![Az Azure Functions futásidejű Preview telepítő][1]

1. **Kattintson a Tovább gombra** előzetes túli hello hello telepítő első szakasza
1. Ha mindenképpen olvassa el hello hello feltételeinek **EULA**, **hello jelölőnégyzetet** tooaccept hello feltételeit és **kattintson a Tovább gombra** tooadvance.
1. Most válassza hello szerepkörök szeretné-e ezen a gépen tooinstall **funkciók szerepkör** és/vagy **funkciók feldolgozói szerepkör** és **kattintson a Tovább gombra**

    ![Az Azure Functions futásidejű Preview Installer - szerepkör kiválasztása][3]

    > [!NOTE]
    > Hello telepítése **funkciók feldolgozói szerepkör** a sok más gépek toodo Igen, kövesse ezeket az utasításokat, és csak válassza **funkciók feldolgozói szerepkör** hello telepítője.

1. **Kattintson a Tovább gombra** toohave hello **Azure Functions futásidejű telepítő** telepítése a számítógépre.
1. Ha teljes hello telepítő indulnak hello **Azure Functions futásidejű konfigurációs eszköz**.

    ![Az Azure Functions futásidejű Preview telepítő befejezése][5]

    > [!NOTE]
    > Ha telepíti az **Windows 10** és hello **tároló** funkció nem korábban engedélyezve van, hello **Azure Functions Futtatókörnyezettel** Installer tooreboot kéri a gép toocomplete hello telepítése.

## <a name="configure-hello-azure-functions-runtime"></a>Hello Azure Functions Futtatókörnyezettel konfigurálása

toocomplete hello Azure Functions Futtatókörnyezettel telepítési hello konfigurációs el kell végeznie.

1. Hello **Azure Functions futásidejű konfigurációs eszköz** jeleníti meg, mely szerepkörök sincs telepítve a számítógépen.

    ![Az Azure Functions futásidejű Preview konfigurációs eszköz][6]

1. Kattintson a hello **adatbázis** lapra, adja meg a hello **kapcsolódási adatait. az SQL Server-példány** és **kattintson az alkalmaz**.  Ez a sorrend toohello Azure Functions Futtatókörnyezettel toocreate egy adatbázis toosupport hello futásidejű szükséges.
    
    ![Az Azure Functions futásidejű Preview adatbázis konfigurációja][7]

1. Kattintson a hello **hitelesítő adatok** fülre.  Ezen a képernyőn létre kell hoznia két új hitelesítő adatokat a fájlmegosztás az Azure Functions üzemeltetéséhez.  **Adja meg a felhasználónevet és jelszót** hello kombinációk **megosztás fájltulajdonos** és hello **fájl megosztási felhasználói** kattintson **alkalmaz**.

    ![Az Azure Functions futásidejű Preview hitelesítő adatok][8]

1. Kattintson a hello **fájlmegosztás** fülre.  Ez a képernyő meg kell adnia hello hello részleteit **fájlmegosztás helye**.  Ez az Ön hozható létre, vagy egy meglévő fájlmegosztást, és kattintson a **alkalmaz**.  Ha egy új fájlmegosztási helyet választja, meg kell hello Azure Functions Futtatókörnyezettel egy könyvtárat.
    
    ![Az Azure Functions futásidejű Preview fájlmegosztás][9]

1. Kattintson a hello **IIS** fülre.  Ezen a lapon látható hello webhelyek hello részleteit az IIS-ben, hogy hello Azure Functions Runtime telepítésének hoz létre.  **Kattintson az alkalmaz** toocomplete.

    ![Az Azure Functions futásidejű előzetes IIS][10]

1. Kattintson a hello **szolgáltatások** fülre.  Ezen a lapon az Azure Functions Futtatókörnyezettel telepítés hello szolgáltatások hello állapotát jeleníti meg.  Ha hello kezdeti konfigurálása után **Azure Functions gazdaszolgáltatás aktiválási** nem fut kattintson **szolgáltatás indítása**

    ![Az Azure Functions futásidejű Preview konfigurációs befejezése][11]

1. Végül Tallózás toohello **Azure Functions futásidejű portálon** ,`https://<machinename>/`

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