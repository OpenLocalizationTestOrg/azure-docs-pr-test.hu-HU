



Attól függően, hogy a környezet és a választási lehetőségek hello parancsfájl hozhat létre minden hello fürt infrastruktúráról, beleértve a hello Azure-beli virtuális hálózat, storage-fiókok, felhőszolgáltatások, tartományvezérlő, helyi vagy távoli SQL-adatbázisok, átjárócsomópont és további fürt csomópontok. Azt is megteheti hello parancsfájl használja a már meglévő Azure-infrastruktúra és létrehozása csak hello HPC-fürt csomópontja.

Egy HPC Pack fürt tervezésével kapcsolatos háttér-információkat, lásd: hello [a termék próbaváltozata és tervezése](https://technet.microsoft.com/library/jj899596.aspx) és [bevezetés](https://technet.microsoft.com/library/jj899590.aspx) az hello HPC Pack 2012 R2 TechNet könyvtár tartalma.

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés**: előfizetés vagy hello Azure globális vagy Azure Kína szolgáltatás használhatja. Az előfizetési korlátozásait hello számát és típusát telepítése fürtcsomópontok hatással. További információ: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../articles/azure-subscription-service-limits.md).
* **Az Azure PowerShell 0.8.10 Windows-ügyfélszámítógéppel, vagy később telepített és konfigurált**: lásd: [Ismerkedés az Azure PowerShell](/powershell/azureps-cmdlets-docs) a telepítési utasításokat és lépéseket tooconnect tooyour Azure-előfizetés.
* **HPC Pack IaaS telepítési parancsfájl**: Töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Verzióellenőrzés hello hello parancsfájl futtatásával `New-HPCIaaSCluster.ps1 –Version`. Ez a cikk hello parancsfájl 4.5.2-es verziójának alapul.
* **Parancsfájl konfigurációs fájlja**: hozzon létre egy XML-fájlt, hogy a parancsfájl hello tooconfigure hello HPC-fürt használja. Ehhez útmutatást és példákat lásd a cikkben később, és a fájl, amely a telepítési parancsfájl hello társul Manual.rtf hello.

## <a name="syntax"></a>Szintaxis
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Hello parancsfájl futtatása rendszergazdaként.
> 
> 

### <a name="parameters"></a>Paraméterek
* **ConfigFile**: hello konfigurációs fájl toodescribe hello HPC-fürt hello fájl elérési útját adja meg. Lásd: hello konfigurációs fájl ebben a témakörben vagy hello parancsfájl tartalmazó hello mappában Manual.rtf hello fájlban olvashat.
* **AdminUserName**: hello felhasználónevét határozza meg. Hello tartományerdő hello parancsfájl hozta létre, ha ez lesz az összes virtuális gép hello helyi rendszergazdai felhasználónév és hello tartomány-rendszergazdai nevet. Ha hello tartomány erdő már létezik, ez adja meg, hello tartományi felhasználó helyi rendszergazdai felhasználói név tooinstall HPC Pack hello.
* **AdminPassword**: hello rendszergazdai jelszót ad meg. Ha nincs megadva a parancssorban hello, hello parancsfájl kéri tooinput hello jelszót.
* **HPCImageName** (nem kötelező): meghatározza a hello HPC Pack VM lemezképnév használt toodeploy hello HPC-fürtben. A Microsoft által biztosított HPC Pack lemezkép hello Azure piactér kell lennie. Ha nincs megadva (ajánlott általában), hello parancsfájl úgy dönt, legújabb közzétett hello [HPC Pack 2012 R2 rendszerképnél](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). hello legújabb lemezkép a Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 Update 3 telepítve alapul.
  
  > [!NOTE]
  > Központi telepítés sikertelen lesz, ha nem adja meg egy érvényes HPC Pack lemezképet.
  > 
  > 
* **Naplófájl** (nem kötelező): hello telepítési naplófájl elérési útjának megadása. Ha nincs megadva, hello parancsfájl hello temp könyvtárában hello futtató hello parancsfájl létrehoz egy naplófájlt.
* **Kényszerített** (nem kötelező): minden hello megerősítő kérdések.
* **NoCleanOnFailure** (nem kötelező): Adja meg a rendszer nem távolítja el az adott hello Azure virtuális gépeken, amelyek telepítése nem sikerült. Hello parancsfájl toocontinue hello központi telepítés ismételt futtatása előtt manuálisan távolítsa el a virtuális gépeken, vagy hello telepítés sikertelen lehet.
* **PSSessionSkipCACheck** (nem kötelező): minden felhőalapú szolgáltatás, a parancsfájl által központilag telepített virtuális gépek, az Azure által automatikusan generált önaláírt tanúsítványt, és minden hello virtuális gépek hello a felhőszolgáltatásban található tanúsítvány használata alapértelmezett Windows hello A távoli felügyeleti (webszolgáltatások WinRM) tanúsítvány. toodeploy HPC-szolgáltatások az Azure virtuális gépeken, alapértelmezés szerint hello parancsfájl átmenetileg telepíti ezeket a tanúsítványokat a helyi számítógép hello\\hello ügyfél számítógép toosuppress megbízható legfelső szintű hitelesítésszolgáltatók tárolójában hello "nem megbízható hitelesítésszolgáltató" biztonsági Hiba történt a parancsfájl végrehajtása közben. hello tanúsítványok hello parancsfájl befejezése után törlődnek. Ha ez a paraméter van megadva, hello tanúsítványok hello ügyfélszámítógépen nincs telepítve, és hello biztonsági figyelmeztetés nem jelenik meg.
  
  > [!IMPORTANT]
  > Ez a paraméter nem ajánlott az üzemi környezetek.
  > 
  > 

### <a name="example"></a>Példa
hello alábbi példa fürtöt hoz létre egy HPC Pack a konfigurációs fájl használata *MyConfigFile.xml*, és adja meg a rendszergazdai hitelesítő adatok hello fürt telepítéséhez.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Néhány fontos megjegyzés
* hello parancsfájl engedélyezheti a feladat elküldése hello HPC Pack webes portál vagy hello HPC Pack REST API-n keresztül.
* hello opcionálisan parancsfájlt egyéni előtti és utáni konfigurációs parancsfájlok hello átjárócsomópont Ha szeretné, hogy tooinstall további szoftvereket vagy más beállítások konfigurálása.

## <a name="configuration-file"></a>Konfigurációs fájl
hello konfigurációs hello telepítési parancsfájl fájlja egy XML-fájlt. hello sémafájl HPCIaaSClusterConfig.xsd hello HPC Pack IaaS telepítési parancsfájl mappában van. **IaaSClusterConfig** hello fájl hello telepítési parancsfájl mappában Manual.rtf részletes leírását lásd hello gyermekelemet tartalmaz hello konfigurációs fájl gyökérelemének hello van.

