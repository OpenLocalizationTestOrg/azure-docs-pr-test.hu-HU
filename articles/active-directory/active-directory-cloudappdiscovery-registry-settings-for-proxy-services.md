---
title: "Felderítési beállításjegyzék beállításainak proxyszolgáltatást aaaCloud |} Microsoft Docs"
description: "hello Ez a témakör célja tooprovide hello kapcsolatos lépéseket kell tooperform tooset szükséges hello port hello hello a Cloud App Discovery-ügynököt futtató számítógépeken."
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
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery beállításjegyzék-beállítások Proxy szolgáltatások
Alapértelmezés szerint a konfigurált toouse csak hello portok 80-as vagy 443-as található hello a Cloud App Discovery-ügynök. Ha azt tervezi, a Cloud App Discovery telepítése által használt egyéni portot (80-as, sem 443-as) proxykiszolgálóval környezetben, szükséges tooconfigure az ügynökök toouse ezt a portot. hello konfigurálása egy beállításkulcs megadásával alapján történik.

hello Ez a témakör célja tooprovide hello kapcsolatos lépéseket kell tooperform tooset szükséges hello port hello hello a Cloud App Discovery-ügynököt futtató számítógépeken.

**toomodify hello port hello a Cloud App Discovery-ügynököt futtató hello számítógép által használt hajtsa végre a következő lépéseket hello:**

1. Hello beállításjegyzék-szerkesztő elindításához. <br> ![Futtassa a következőt:](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Keresse meg tooor létrehozása a következő beállításkulcs hello: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint** 
3. Hozzon létre egy új **karakterláncsoros** nevű értéket **portok**. ![Új](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. tooopen hello **Karakterláncsor szerkesztése** párbeszédpanel, kattintson duplán a hello portok érték.
5. Hello érték adatok szövegmezőben írja be a következő értékek hello, és adja hozzá a szervezet által használt összes egyéni portokat: <br><br>
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
6. Kattintson a **OK** tooclose hello **Karakterláncsor szerkesztése** párbeszédpanel.

**További források**

* [Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül](active-directory-cloudappdiscovery-whatis.md) 

