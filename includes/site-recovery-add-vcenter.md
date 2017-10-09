* A **vCenter hozzáadása**, adjon meg egy rövid nevet hello vSphere gazdagép vagy a vCenter-kiszolgálóhoz, és adja meg a hello IP-cím vagy hello kiszolgáló teljes Tartományneve. Hello port hagyja 443-as, kivéve, ha a VMware Server kérések egy másik portra beállított toolisten. Válassza ki a hello fiók, amely tooconnect toohello VMware vCenter vagy vSphere ESXi-kiszolgálón. Kattintson az **OK** gombra.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Hello VMware vCenter-kiszolgálót vagy egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal hello vCenter vagy a gazdagép server VMware vSphere-gazdagép hozzáadása, győződjön meg arról, hogy hello fiókja rendelkezik-e ezek a jogosultságok engedélyezve: Datacenter, Datastore, mappa, a gazdagép, hálózati , Erőforrás, a virtuális gép és a vSphere elosztott kapcsoló. Emellett hello VMware vCenter kiszolgálót kell hello tárolási nézetek jogosultság engedélyezve van.
