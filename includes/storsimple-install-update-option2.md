<!--author=SharS last changed: 03/17/2016-->

#### <a name="tooinstall-update-12-from-hello-azure-classic-portal"></a>a klasszikus Azure portálon hello 1.2-es frissítés tooinstall
1. A klasszikus Azure portálon hello, válassza a toohello **eszközök** lapon, és válassza ki azt az eszközt.
2. Keresse meg a túl**eszközök** > **konfigurálása**.
3. A **hálózati illesztők**, először ellenőrizze, hogy rendelkezik-e legalább egy hálózati adapter, amely az iSCSI-kompatibilis. Keresse meg hello hálózati illesztőt (mint a DATA 0), amely rendelkezik hozzárendelt átjáró.
4. Tiltsa le a hello hálózati adapter, amelyen egy hozzárendelt átjáró, és mentse a módosított konfigurációs hello. Vegye figyelembe a hello hálózati kapcsolati beállítások megmaradnak, és ezért ha újra engedélyezi a hálózati illesztő később, hello portal visszaállítja toohello eredeti beállításait.
5. Most [hello Azure klasszikus portál tooinstall 1.2-es frissítés használja](#install-update-12-via-the-azure-classic-portal). Ez az eljárás 3. lépés-től kezdődő hello utasításokat kövesse. Minden hello frissítés telepítése után engedélyezheti újra letiltott hello hálózati illesztőt.

