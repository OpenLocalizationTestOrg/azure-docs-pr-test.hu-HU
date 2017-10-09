hello storage emulator megosztott kulcsos hitelesítést támogatja egy rögzített fiókhoz és egy jól ismert hitelesítési kulcs. A fiók és a kulcs nem hello csak megosztott kulcsos hitelesítő adatok hello storage emulator való használatra engedélyezett. Ezek a következők:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> hello storage emulator által támogatott hello hitelesítési kulcs csak tesztelési hello funkcióit az ügyfél-hitelesítési kód készült. Bármilyen biztonsági célt nem szolgál. A termelési tárfiók és a kulcs hello storage emulator nem használható. Ne használjon hello fejlesztői fiók termelési adatokkal.
> 
> hello storage emulator csak a HTTP Protokollon keresztül kapcsolatot támogat. Azonban a HTTPS protokoll egy éles Azure-tárfiók-erőforrások eléréséhez ajánlott hello.
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a>Csatlakozás parancsikonnal toohello emulátor fiók
hello legegyszerűbb módja tooconnect toohello storage emulator az alkalmazásról egy kapcsolati karakterláncot az alkalmazás konfigurációs fájljában hello helyi hivatkozó tooconfigure `UseDevelopmentStorage=true`. A kapcsolati karakterlánc toohello a storage emulatort, például egy *app.config* fájlt: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a>Csatlakozás toohello emulátor fiókját hello jól ismert fióknevet és kulcsot
toocreate egy kapcsolati karakterláncot, hogy hivatkozásokat hello emulátor fióknevet és kulcs, meg kell adnia hello végpontok egyes hello szolgáltatási meg akarja toouse hello emulátorától hello kapcsolat-karakterláncban. Erre akkor szükség, így hello kapcsolati karakterlánc használatával hello emulátor végpontok, amelyek eltérnek a termelési tárfiókon hivatkozik. Például a kapcsolati karakterlánc értékét hello fog kinézni:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Az értéket nem látható a fenti azonos toohello helyi `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Adjon meg egy HTTP-proxy
Ha a szolgáltatás hello storage emulatorban tesztelést egy HTTP-proxy toouse is megadható. Ez lehet hasznos, ha HTTP-kérések és válaszok betartásával, akkor hibakeresése hello tárolószolgáltatások műveleteket közben. a proxy toospecify hozzáadása hello `DevelopmentStorageProxyUri` toohello kapcsolati karakterlánc lehetőséget, és állítsa be az érték toohello proxy URI. Például: Itt toohello storage emulator mutat, és konfigurálja a HTTP-proxy kapcsolati karakterlánc:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

