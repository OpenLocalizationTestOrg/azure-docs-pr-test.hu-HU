---
title: "Active Directory Domain Services: Az Active Directory Domain Services engedélyezése | Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 6263eb1849808a7c85e572e1046bc9039362dd9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával

## <a name="task-3-enable-azure-active-directory-domain-services"></a>3. feladat: Az Azure Active Directory Domain Services engedélyezése
Ebben a feladatban engedélyezi Azure Active Directory tartományi szolgáltatások (az Azure AD DS) a címtáron a hello lépések végrehajtásával:

1. Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali ablaktáblában jelöljön ki hello **Active Directory** gombra.
3. Válassza ki a kívánt Azure Active Directory tartományi szolgáltatások tooenable Azure Active Directory (Azure AD) hello bérlőt (címtárat).

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. A hello **preview directory** hello kattintson **konfigurálása** fülre.

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. A **tartományi szolgáltatások**, hello módosítása **engedélyezése tartományi szolgáltatásokat a címtárhoz** beállítás túl**Igen**.  
    További Azure Active Directory tartományi szolgáltatások konfigurációs beállítások hello oldalon jelennek meg.

    ![Tartományi szolgáltatások engedélyezése](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Ha engedélyezi az Azure Active Directory tartományi szolgáltatások a bérlő számára, az Azure AD hoz létre, és hello Kerberos és NTLM hitelesítő adatok kivonatait felhasználók hitelesítéséhez szükséges tárolja.
   >
   >
6. Adja meg a hello **DNS-tartománynevet a tartományi szolgáltatások**.

   * hello hello címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint engedélyezett.

   * hello lista tartalmazza az Azure AD címtárhoz konfigurált összes tartomány, többek között, mindkét ellenőrzése és a nem ellenőrzött tartományok hello konfigurált **tartományok** fülre.

   * Egy egyéni tartománynevet is megadhat. Ebben a példában hello egyéni tartománynév megadása *contoso100.com*.

     > [!WARNING]
     > a megadott tartománynév előtagja hello (például *contoso100* a hello *contoso100.com* tartománynevet) kell tartalmaznia a 15 karaktert. Nem hozhat létre olyan Active Directory Domain Services tartományt, amelynek az előtagja 15 karakternél többet tartalmaz.
     >
     >
7. Győződjön meg arról, hogy hello DNS-tartománynevet választott hello felügyelt tartomány már nem létezik hello virtuális hálózatban. Pontosabban ellenőrizze hogy toosee:

   * Már létezik hello tartomány hello virtuális hálózaton azonos DNS-tartománynevet.

   * hello választott virtuális hálózathoz van a helyszíni hálózat VPN-kapcsolattal, és a tartomány hello rendelkezik a helyszíni hálózaton azonos DNS-tartománynevet.

   * Hello virtuális hálózaton van ilyen nevű létező felhőalapú szolgáltatást.
8. Válasszon egy virtuális hálózatot kívánja Azure Active Directory tartományi szolgáltatások toobe érhető el. Válassza ki a hello virtuális hálózat és a dedikált alhálózati hello létrehozott **Connect tartományszolgáltatási toothis virtuális hálózati** legördülő listából. Is vegye figyelembe a következőket hello:

   * Győződjön meg arról, Ön által megadott hello virtuális hálózat tartozik tooan Azure-régió, Azure Active Directory tartományi szolgáltatások által támogatott. tooascertain hello Azure-régiók Azure Active Directory tartományi szolgáltatások esetén érhető el, lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).

   * Virtuális hálózatok tartozó tooa régió, ahol nem támogatott az Azure Active Directory tartományi szolgáltatások nem hello legördülő listában jelennek meg.

   * Használjon egy dedikált alhálózati hello virtuális hálózaton belül az Azure Active Directory tartományi szolgáltatásokhoz. Tegye *nem* válasszon hello átjáró-alhálózatot. Lásd: [hálózati szempontok](active-directory-ds-networking.md).

   * Hasonlóképpen Azure Resource Manager használatával létrehozott virtuális hálózatok nem hello legördülő listában jelennek meg. A Resource Manager-alapú virtuális hálózatokat az Active Directory Domain Services jelenleg nem támogatja.
9. Azure Active Directory tartományi szolgáltatások, tooenable hello munkaablakban hello lapon hello alján kattintson **mentése**.
    * Azure Active Directory tartományi szolgáltatások engedélyezése a címtáron van, közben hello lap mezőjében a *függőben lévő*.

        ![A Tartományi szolgáltatások engedélyezése ablak](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Az Active Directory Domain Services biztosítja a felügyelt tartományok magas rendelkezésre állását. Miután engedélyezte az Azure Active Directory tartományi szolgáltatások, a tartományi szolgáltatások elérhetőek a virtuális hálózati hello hello IP-címek megjelenített egyszerre csak egy. hello második IP-cím jelenik meg hamarosan hello után először, amint hello szolgáltatás lehetővé teszi, hogy magas rendelkezésre állást a tartományra. Ha magas rendelkezésre állás konfigurálva van és aktív a tartományon, megjelenítheti a hello két IP-címek **tartományi szolgáltatások** hello szakasza **konfigurálása** fülre.
        >
        >
    * Too30 körülbelül 20 perc elteltével hello tartományi szolgáltatások érhetők el a virtuális hálózaton hello az első IP-cím **IP-cím** található hello **konfigurálása** lap.

        ![A Tartományi szolgáltatások ablak, rajta az első kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Ha magas rendelkezésre állás a tartomány működési, két IP-címek hello oldalon jelennek meg. A felügyelt tartomány a kiválasztott virtuális hálózaton ezen a két IP-címen érhető el.

10. Vegye figyelembe a hello két IP-címet, hogy a virtuális hálózat hello DNS-beállítások segítségével frissítheti. Így lehetővé teszi, hogy a virtuális gépek hello virtuális hálózati tooconnect toohello tartományban műveletek, például a tartományhoz való csatlakozást.

    ![A Tartományi szolgáltatások ablak, rajta a két kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Attól függően, hogy az Azure AD-bérlő (például hello száma felhasználókat és csoportokat) hello méretét a szinkronizálás tooyour által kezelt tartomány eltart egy ideig. Ez a szinkronizálási folyamat hello háttérben történik. Az objektumok tízezreit tartalmazó nagy bérlők egy vagy két összes felhasználó, csoporttagságot és hitelesítő adatok toobe szinkronizált napot is eltarthat.
>
>

## <a name="next-step"></a>Következő lépés
[4. feladat: hello hello Azure-beli virtuális hálózat DNS-beállításainak frissítése](active-directory-ds-getting-started-update-dns.md)
