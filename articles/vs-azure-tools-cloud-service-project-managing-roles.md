---
title: "a felhőalapú szolgáltatások a Visual Studio aaaManaging szerepkörök az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd és eltávolítása szerepkörök az Azure felhőszolgáltatások Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 131edc534d1110ba3d25cd00a3a24b643576875c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Az Azure felhőszolgáltatások Visual Studio szerepkörök kezelése
Miután létrehozta az Azure felhőszolgáltatást, adja hozzá az új szerepkörök tooit, vagy meglévő szerepkörök eltávolításához. Egy meglévő projektjébe importálhatja és alakítsa át a tooa szerepkör is. Például egy ASP.NET-webalkalmazás importálhatja és kijelöl egy webes szerepkörben.

## <a name="adding-a-role-tooan-azure-cloud-service"></a>Egy szerepkör tooan Azure-felhőszolgáltatásban hozzáadása
a lépéseket követve hello végigvezeti Önt egy webes vagy feldolgozói szerepkör tooan Azure felhőszolgáltatás-projekt hozzáadása a Visual Studio.

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projektcsomópontra

1. Kattintson a jobb gombbal hello **szerepkörök** csomópont toodisplay hello helyi menü. Hello helyi menüből válassza ki a **Hozzáadás**, majd válasszon egy meglévő webes szerepkör vagy a feldolgozói szerepkör hello aktuális megoldás, vagy hozzon létre egy webes vagy feldolgozói szerepkör projekt. Is válassza ki a megfelelő projektben, például egy ASP.NET webalkalmazás projekthez, és társíthatja egy szerepkör-projektet.

    ![Menü Beállítások tooadd egy szerepkör tooan Azure felhőszolgáltatás-projekt](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>A szerepkör eltávolítását Azure-felhőszolgáltatás
hello következő lépések végigvezetik a Visual Studio egy Azure-felhőszolgáltatás-projekt egy webes vagy feldolgozói szerepkör eltávolítását.

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projektcsomópontra

1. Bontsa ki a hello **szerepkörök** csomópont.

1. Kattintson a jobb gombbal hello csomópont tooremove, és, hello helyi menüből jelöljük ki **eltávolítása**. 

    ![Menü Beállítások tooadd egy szerepkör tooan Azure-felhőszolgáltatás](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a>Olvasása a következő egy szerepkör tooan Azure felhőszolgáltatás-projekt
Ha a szerepkör eltávolítása a felhőszolgáltatás-projekt, de később úgy dönt, tooadd hello biztonsági szerepkör toohello projekt, csak hello szerepkör nyilatkozat és alapvető attribútumait, például a végpontok és diagnosztikai információk, kerülnek. További erőforrások vagy hivatkozások kerülnek toohello `ServiceDefinition.csdef` fájl vagy toohello `ServiceConfiguration.cscfg` fájlt. Ha azt szeretné, tooadd ezt az információt, toomanually kell adja hozzá ezeket a fájlokat programba.

Például előfordulhat, hogy távolítsa el a webes szerepkör-szolgáltatás, és később mellett dönt a szerepkör biztonsági tooadd megoldásba történő. Ha így tesz, akkor hiba történik. tooprevent Ez a hiba tooadd hello rendelkezik `<LocalResources>` elem látható a következő hello XML vissza az hello `ServiceDefinition.csdef` fájlt. Használjon hello neve hello webes szerepkör-szolgáltatás hello hello name attribútum részeként vissza hello projektbe hozzáadott  **<LocalStorage>**  elemet. Ebben a példában hello webszolgáltatási szerepkörének hello neve nem **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Következő lépések
- [A Visual Studio hello szerepkörök az Azure-felhőszolgáltatás konfigurálása](vs-azure-tools-configure-roles-for-cloud-service.md)
