---
title: "Azure Automation-fiók aaaManage |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomanage hello például a tanúsítvány megújításához, törlés és helytelen konfigurálása az Automation-fiók konfigurációját."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a>Azure Automation-fiók kezelése
Egy bizonyos ponton az Automation-fiók lejárta előtt toorenew hello tanúsítványt kell. Ha úgy véli, hogy a Futtatás mint fiók hello feltörték, törlése, és hozza létre újból. Ez a szakasz ismerteti hogyan tooperform ezeket a műveleteket.

## <a name="self-signed-certificate-renewal"></a>Önaláírt tanúsítvány megújítása
hello önaláírt tanúsítványt, amelyet létrehozott hello futtató fiók létrehozásának dátuma hello egy év lejár. A tanúsítványt bármikor meg lehet újítani a lejárata előtt. Amikor megújítja, a hello aktuális érvényes tanúsítványra, hogy legfeljebb vagy aktívan futó ütemezett, és az, hogy azokkal hello Futtatás mint fiókot, runbook érintett nem negatív megtartott tooensure. amíg a lejárati dátum nem érvényes marad hello tanúsítványt.

> [!NOTE]
> Ha konfigurálta az Automation Futtatás mint fiók toouse a vállalati hitelesítésszolgáltató által kiállított tanúsítványt, és ezt a beállítást használja, hello vállalati tanúsítvány helyébe egy önaláírt tanúsítványt.

toorenew hello tanúsítvány, a következő hello:

1. Hello Azure-portálon nyissa meg a hello Automation-fiók.

2. A hello **Automation-fiók** paneljén, hello **tulajdonságok fiók** ablaktáblán, a **Fiókbeállítások**, jelölje be **futtató fiókok**.

    ![Az Automation-fiók tulajdonságpanelje](media/automation-manage-account/automation-account-properties-pane.png)
3. A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy hello klasszikus Futtatás mint fiókot, amelyet toorenew hello tanúsítványt.

4. A hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **tanúsítvány megújítása**.

    ![Futtató fiók tanúsítványának megújítása](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Hello tanúsítvány megújítása folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Futtató fiók vagy klasszikus futtató fiók törlése
Ez a szakasz ismerteti, hogyan toodelete, majd hozza létre a Futtatás mint vagy a klasszikus futtató fiókot. Ez a művelet végrehajtásakor hello Automation-fiók őrződnek meg. A Futtatás mint vagy a klasszikus futtató fiók törlése után újra létrehozhatja a hello Azure-portálon.

1. Hello Azure-portálon nyissa meg a hello Automation-fiók.

2. A hello **Automation-fiók** hello fiók tulajdonságok panelen, jelölje be a panelt **futtató fiókok**.

3. A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy a klasszikus futtató fiókot, amelyet az toodelete. Ezután a hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **törlése**.

 ![Futtató fiók törlése](media/automation-manage-account/automation-account-delete-runas.png)

4. Hello fiók törlése folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.

5. Hello fiók törlése után újra létrehozhatja a hello **futtató fiókok** hello kiválasztásával a Tulajdonságok panelére léphet létrehozása beállítást **Azure futtató fiók**.

 ![Hozza létre újból hello Automation futtató fiók](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Hibás konfiguráció
Néhány hello futtató vagy a klasszikus futtató fiók toofunction szükséges konfigurációs elemek megfelelő lehet, hogy törölték vagy helytelenül van létrehozva a kezdeti beállítás során. hello elemek a következők:

* Tanúsítványobjektum
* Kapcsolatobjektum
* A Futtatás mint fiók hello közreműködői szerepkör eltávolították.
* Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben

Az előző hello és helytelen konfigurálása más példányai, hello Automation-fiók észleli hello változik, és egy állapotát jeleníti meg az *hiányos* a hello **futtató fiókok** hello a Tulajdonságok panelen fiók.

![Hiányos futtatófiók-konfigurációs állapot](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Hello Futtatás mint fiók kiválasztásakor hello fiók **tulajdonságok** ablaktáblán jelennek meg a következő hibaüzenet hello:

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

A Futtatás mint fiók problémák gyorsan megoldható törli, majd újra létrehozza a hello fiók.

## <a name="next-steps"></a>Következő lépések
* Szolgáltatásnevekről kapcsolatos további információkért tekintse meg túl[alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).
* Szerepköralapú hozzáférés-vezérlés az Azure Automationben kapcsolatos további információkért tekintse meg túl[szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).
* Tanúsítványok és az Azure-szolgáltatásokkal kapcsolatos további információkért tekintse meg túl[tanúsítványok áttekintése Azure-szolgáltatásokhoz](../cloud-services/cloud-services-certs-create.md).
