---
title: "Cloud App Discovery beállításjegyzék-beállítások proxyszolgáltatást |} Microsoft Docs"
description: "Ez a témakör célja biztosítja, hogy a lépéseket kell elvégeznie a szükséges port beállítása a Cloud App Discovery-ügynököt futtató számítógépeken."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery beállításjegyzék-beállítások Proxy szolgáltatások
Alapértelmezés szerint a Cloud App Discovery-ügynök csak a portok 80-as vagy 443-as használatára van konfigurálva. Ha azt tervezi, a Cloud App Discovery telepítése által használt egyéni portot (80-as, sem 443-as) proxykiszolgálóval környezetben, akkor kell konfigurálni az ügynököket, a port használatára. A konfiguráció egy beállításkulcs megadásával alapul.

Ez a témakör célja biztosítja, hogy a lépéseket kell elvégeznie a szükséges port beállítása a Cloud App Discovery-ügynököt futtató számítógépeken.

**Módosítsa a portot használják a Cloud App Discovery-ügynököt futtató számítógépről, hajtsa végre az alábbi lépéseket:**

1. A beállításjegyzék-szerkesztő elindításához. <br> ![Futtassa a következőt:](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Keresse meg, vagy hozza létre a következő beállításkulcsot: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint** 
3. Hozzon létre egy új **karakterláncsoros** nevű értéket **portok**. ![Új](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Lehetőségre a **Karakterláncsor szerkesztése** párbeszédpanel, kattintson duplán a portok értékre.
5. Az érték adatok szövegmezőben írja be a következő értékeket, és adja hozzá a szervezet által használt összes egyéni portokat: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Karakterláncsor szerkesztése](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Kattintson a **OK** bezárásához a **Karakterláncsor szerkesztése** párbeszédpanel.

**További források**

* [Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül](active-directory-cloudappdiscovery-whatis.md) 

