Ebben a lépésben egy ügyfélalkalmazást, hogy fut-hello segítségével tesztelnie hello rendelkezésre állási csoport figyelőjének ugyanazon a hálózaton.

Ügyfélkapcsolat-követelményeknek hello rendelkezik:

* Ügyfél kapcsolatok toohello figyelő megtalálható egy másik felhőalapú szolgáltatást, mint egy hello, hogy a gazdagépek Always On rendelkezésre állási másodpéldányok hello gépek kell származnia.
* Ha külön alhálózatokon vannak hello mindig a replikákat, ügyfeleknek meg kell adniuk *MultisubnetFailover = True* hello kapcsolati karakterláncban. Ez a feltétel eredményezi, párhuzamos kapcsolat megpróbál tooreplicas hello a különböző alhálózatokon. Ez az eset tartalmazza a kereszt-régió Always On rendelkezésre állási csoport központi telepítése.

Egy példa: tooconnect toohello figyelő hello a virtuális gépek hello egyikéből azonos Azure virtuális hálózat (de nem egy, a replikát tartalmazó). Egy egyszerű módot toocomplete Ez a vizsgálat tootry tooconnect SQL Server Management Studio toohello rendelkezésre állási csoport figyelőjének. Egy másik egyszerű módszer a következő: toorun [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx), az alábbiak szerint:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Hello EndpointPort érték esetén *1433*, nem szükséges toospecify áll azt hello hívásban. hello korábbi hívás is feltételezi, hogy hello ügyfél gép illesztett toohello ugyanabban a tartományban, és a hello hívó rendelkezik engedéllyel a hello adatbázis Windows-hitelesítés használatával.
> 
> 

Hello figyelő tesztelésekor lehetnek meg arról, hogy toofail hello rendelkezésre állási csoport toomake meg arról, hogy az ügyfélszámítógépek csatlakozhatnak-e toohello figyelő feladatátvételek között.

