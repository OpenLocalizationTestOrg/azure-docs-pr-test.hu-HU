---
title: "aaaChange hello Azure Active Directory-bérlőt az Azure Remoteappban |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange hello Azure Active Directory-bérlőhöz tartozó Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 20faf169-6e48-428a-8bdd-f231daff19fa
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d0928b099b7fcfb3ab16077e295d7aaf519c3653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-azure-active-directory-tenant-in-azure-remoteapp"></a>Az Azure Remoteappban hello Azure Active Directory-bérlő módosítása
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp használja az Azure Active Directory (Azure AD) tooallow felhasználói hozzáférést. az Azure Remoteappban használható hello csak az Azure AD-bérlő egy hello Azure-előfizetéssel társított hello. Hello tartozó előfizetés megtekintheti a hello **beállítások** hello lapjára. Nézze meg hello **Directory** hello oszlopa **előfizetések** fülre.

> [!NOTE]
> Az e módosítás toosucceed először távolítson el minden felhasználó elemet hello meglévő Azure Active Directory-bérlőt az összes Azure RemoteApp-gyűjtemény. toodo e, lépjen toohello Azure-portálon lépjen toohello **Azure RemoteApp** lapra, majd nyissa meg a minden Azure RemoteApp-gyűjteményt. Nyissa meg toohello **felhasználók** fülre, és távolítsa el a felhasználók tooyour aktuális Azure Active Directory-bérlő tartoznak. Ismételje meg az összes meglévő Azure RemoteApp-gyűjteményt. E nélkül nem fog tudni toocreate vagy javítás gyűjteményeket.
> 
> 

A különböző bérlők toouse, használja az előfizetés és ezen lépéseket toochange hello társításának:

1. Hello portálon, minden Azure AD-felhasználók toowhich eltávolítása biztosított hozzáférés tooAzure RemoteApp-gyűjteményére. (Hello Megjegyzés fölött lépéseket meg, hogyan toodo ez.)
2. Állítsa be a Microsoft-fiók (korábban Live ID) hello szolgáltatás-rendszergazdát. (Ne tudják, ha Ön már hello szolgáltatás-rendszergazdák? Azt is megtudhatja, kattintva **beállítások -> rendszergazdák**.) Most ez hogyan módosítja, amely:
   
   1. Kattintson a hello felhasználói hello jobb felső sarokban, majd **megtekintése a számlázási**.
   2. Kattintson a hello előfizetésre. Ezt követően az hello új lapon görgessen lefelé, és kattintson **előfizetés részleteinek szerkesztése** a jobb oldali hello. (Hello középső alsó jobb, ha adott típusú segít találja meg.)
   3. Írja be a Microsoft-fiók hello hello felhasználóhoz, amelyeket hello szolgáltatás rendszergazdájával.
3. Most jelentkezzen ki hello portálon, és jelentkezzen be ismét hello hello előző lépésben megadott Microsoft-fiókkal.
4. Kattintson a **új -> App szolgáltatások -> Active Directory -> a könyvtár -> egyéni létrehozása**.
5. A **Directory**, válassza a **meglévő címtár használata**. Az oktatóanyagban módosítjuk toohave toosign kívül hello portálon, így dönthet **kijelentkezésre készen toobe vagyok**.
6. Jelentkezzen be ismét a hello tooadd kívánt portál hello címtár globális rendszergazdaként. (Sikertelen már globális rendszergazda, ha Ön egy ciklikus a után lesz bejelentkezni, és ezután jelentkezzen ki.)
7. Meg kell adnia, amikor bejelentkezik Ha azt szeretné, toouse a meglévő AD-bérlő az előfizetéshez. Kattintson a **Folytatás**, és kattintson a **azonnali Kijelentkezés**.
8. Jelentkezzen be újra, és térjen vissza túl**beállítások -> előfizetések**. Jelölje ki az előfizetését, és kattintson **könyvtár szerkesztése**. Válassza ki a megjeleníteni kívánt toouse hello Azure AD bérlői.

Most már használhatja hello új Azure AD bérlő toocontrol hozzáférés toohello Azure-előfizetést és tooconfigure felhasználói hozzáférés az Azure Remoteappban.

