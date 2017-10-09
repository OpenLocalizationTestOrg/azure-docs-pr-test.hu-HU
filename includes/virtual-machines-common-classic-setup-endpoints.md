
Minden egyes végpont rendelkezik egy *nyilvános port* és egy *magánhálózati port*:

* hello nyilvános port bejövő forgalom toohello virtuális gépek esetén a hello Internet hello Azure load balancer toolisten használják.
* hello magánhálózati port bejövő forgalmat, általában végcélként tooan alkalmazás vagy szolgáltatás hello virtuális gépen futó virtuális gép toolisten hello használják.

Alapértelmezett értékeinek hello IP-protokoll, és jól ismert hálózati protokollok a TCP vagy UDP-portok kapnak, ha végpontok létrehozásához hello Azure-portálon. Egyéni végpontok toospecify hello helyes IP-protokoll (TCP és UDP) és hello nyilvános vagy privát portokkal lesz szüksége. toodistribute bejövő forgalom véletlenszerűen el több virtuális gépre, szüksége lesz toocreate egy elosztott terhelésű készlet több végpont már nem álló.

Miután létrehozott egy végpontot, használhat egy hozzáférés-vezérlési lista (ACL) toodefine szabályok, amelyek engedélyezése vagy elutasítása bejövő forgalom hello toohello nyilvános port hello végpont a forrás IP-címe alapján. Azonban ha hello virtuális gépet egy Azure virtuális hálózatban, használjon hálózati biztonsági csoportok helyette. További információkért lásd: [hálózati biztonsági csoportok](../articles/virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Az Azure virtuális gépek tűzfal-konfigurálása automatikusan történik, amely automatikusan beállítja Azure távoli kapcsolatot végpontok társított portok. A portok az összes többi végpont a megadott, nem-konfigurálása automatikusan történik toohello tűzfal hello virtuális gép. A végpont hello virtuális gép létrehozásakor kell, hogy a tűzfal hello virtuális gép hello tooensure is lehetővé teszi a hello forgalom hello protokoll és magánhálózati portot megfelelő toohello végpont-konfiguráció. tooconfigure hello tűzfal, hello dokumentációjában vagy a hello operációs rendszer hello virtuális gépen futó online súgóját.
>
>

## <a name="create-an-endpoint"></a>Végpont létrehozása
1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **virtuális gépek**, majd kattintson a hello virtuális gép, amelyet az tooconfigure hello neve.
3. Kattintson a **végpontok** a hello **beállítások** csoport. Hello **végpontok** laplistákhoz hello aktuális végpontjai hello virtuális gépet. (Ez a példa egy Windows virtuális Gépet. A Linux virtuális gép alapértelmezés szerint megjelenik a végpont az SSH.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Végpontok](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. Kattintson a parancssáv hello hello végpont bejegyzések fölött, **Hozzáadás**.
5. A hello **végpont hozzáadása** írja be a hello végpont nevét **neve**.
6. A **protokoll**, válasszon **TCP** vagy **UDP**.
7. A **nyilvános Port**, hello portszámát hello hello Internet érkező bejövő forgalmat. A **magánhálózati Port**, írja be a hello portszám mely hello virtuális gép figyel. Ezek a portszámok eltérő lehet. Győződjön meg arról, hogy hello tűzfal hello virtuális gépen már konfigurált tooallow hello forgalom megfelelő toohello protocol (6. lépés) és magánhálózati portot.
10. Kattintson az **OK** gombra.

új endpoint hello jelenik meg a hello **végpontok** lap.

![Végpont létrehozása sikeresen megtörtént](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-hello-acl-on-an-endpoint"></a>A végpont ACL hello kezelése
a végpont ACL hello toodefine hello azon számítógépek gyűjteménye, amelyek képesek a forgalmat küldeni, korlátozhatja forgalom forrás IP-címe alapján. Kövesse az alábbi lépéseket tooadd, módosíthatja vagy eltávolíthatja a végpont ACL.

> [!NOTE]
> Ha hello végpont egy elosztott terhelésű készlet része, a változtatások vannak végponti ACL toohello alkalmazott tooall végpontok hello készletében.
>
>

Ha hello virtuális gépet egy Azure virtuális hálózatban, ajánlott hálózati biztonsági csoportok hozzáférés-vezérlési listák helyett. További információkért lásd: [hálózati biztonsági csoportok](../articles/virtual-network/virtual-networks-nsg.md).

1. Ha még nem tette meg, jelentkezzen be Azure-portálon toohello.
2. Kattintson a **virtuális gépek**, majd kattintson a hello virtuális gép, amelyet az tooconfigure hello neve.
3. Kattintson a **Végpontok** elemre. Hello listáról válassza ki a megfelelő végpont hello. hello ACL lista alján hello hello van.

   ![Adja meg a hozzáférés-vezérlési lista adatait](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Sorok hello lista tooadd, a delete és a Szerkesztés szabályok használata a hozzáférés-vezérlési Listában, és módosíthatja azok sorrendjét. Hello **távoli alhálózati** értéke egy IP-címtartomány a hello Azure load balancer használ toopermit hello vagy tagadhatnak meg a forrás IP-címe alapján hello forgalom Internet érkező bejövő forgalmat. Lehet, hogy toospecify hello IP-címtartomány CIDR formátumban, más néven a cím előtag formátuma. Példa: `10.1.0.0/8`.

 ![Új ACL-bejegyzéssel](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Szabályok tooallow csak érkező forgalom megfelelő hello Internet vagy toodeny forgalom a meghatározott, ismert címtartományokból tooyour számítógépek adott számítógépekhez használható.

hello szabályok kiértékelése sorrendben az első szabály hello kezdési és befejezési hello utolsó szabállyal. Ez azt jelenti, hogy a szabályok a legkevésbé korlátozó toomost korlátozó lehetnek rendezve. További információt és példákat lásd: [Mi az a hálózati hozzáférés-vezérlési lista](../articles/virtual-network/virtual-networks-acl.md).
