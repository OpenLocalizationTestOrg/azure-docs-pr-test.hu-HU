# [Áttekintés](media-services-overview.md)
## [Forgatókönyvek és rendelkezésre állás](scenarios-and-availability.md)
## [Alapelvek](media-services-concepts.md)

# Bevezetés
## [Fiók létrehozása és felügyelete](media-services-portal-create-account.md)
## [A fejlesztési környezet beállítása](media-services-set-up-computer.md)
### [.NET](media-services-dotnet-how-to-use.md)
### [REST](media-services-rest-how-to-use.md)  
## [API-hozzáférés AAD-hitelesítés használatával](media-services-use-aad-auth-to-access-ams-api.md)
### [AAD-hitelesítés kezelése a portál használatával](media-services-portal-get-started-with-aad.md)
### [API-hozzáférés .NET-tel](media-services-dotnet-get-started-with-aad.md)
### [API-hozzáférés REST-tel](media-services-rest-connect-with-aad.md)
### [AAD-alkalmazás létrehozása és konfigurálása az Azure CLI használatával](media-services-cli-create-and-configure-aad-app.md)
### [AAD-alkalmazás létrehozása és konfigurálása a Azure PowerShell-lel](media-services-powershell-create-and-configure-aad-app.md)

## Igény szerinti videó továbbítása
### [Azure Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Élő közvetítés
### [Azure Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Útmutató
## Kezelés
### Entitások
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Streamvégpontok](media-services-streaming-endpoints-overview.md)
#### [Azure Portal](media-services-portal-manage-streaming-endpoints.md)
#### [.NET](media-services-dotnet-manage-streaming-endpoints.md)
### Tárolás
#### [A Media Services frissítése tárelérési kulcsok váltása után](media-services-roll-storage-access-keys.md)
#### [Adategységek felügyelete több tárfiókban](meda-services-managing-multiple-storage-accounts.md)
### [Kvóták és korlátozások](media-services-quotas-and-limitations.md)

## Tartalom feltöltése
### Fájlok feltöltése egy fiókba
#### [Azure Portal](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Nagy fájlok feltöltése az Asperával](media-services-upload-files-with-aspera.md)
### [Fájlok feltöltése a StorSimple-lel](media-services-upload-files-from-storsimple.md)
### [Meglévő blobok másolása](media-services-copying-existing-blob.md)

## [Tartalom kódolása](media-services-encode-asset.md)
### [Kódolók összehasonlítása](media-services-compare-encoders.md)
### [A kódolás sebességének és egyidejűségének kezelése](media-services-manage-encoding-speed.md)
### Media Encoder Standard (MES)
#### [Media Encoder Standard-formátumok és -kodekek](media-services-media-encoder-standard-formats.md)
#### [Sávszélességi skála automatikus létrehozása a MES használatával](media-services-autogen-bitrate-ladder-with-mes.md)
#### Kódolás a Media Encoder Standard használatával
##### [Azure Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
##### [REST](media-services-rest-encode-asset.md)
#### [Speciális kódolás a MES használatával](media-services-advanced-encoding-with-mes.md)
##### [Media Encoder Standard-beállításkészletek testreszabása](media-services-custom-mes-presets-with-dotnet.md)
##### [Miniatűrök létrehozása a .NET-es Media Encoder Standard használatával](media-services-dotnet-generate-thumbnail-with-mes.md)
##### [Videók körülvágása a Media Encoder Standarddel](media-services-crop-video.md)
#### MES-sémák
##### [Media Encoder Standard-séma](media-services-mes-schema.md)
##### [Bemeneti metaadatok](media-services-input-metadata-schema.md)
##### [Kimeneti metaadatok](media-services-output-metadata-schema.md)
#### [MES-beállításkészletek](media-services-mes-presets-overview.md) 
##### [H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md)
##### [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md)
##### [H264 Multiple Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 16x9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md)
##### [H264 Multiple Bitrate 16x9 for iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md)
##### [H264 Multiple Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md)
##### [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md)
##### [H264 Multiple Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 4x3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md)
##### [H264 Multiple Bitrate 4x3 for iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md)
##### [H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md)
##### [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md)
##### [H264 Single Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md)
##### [H264 Single Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md)
##### [H264 Single Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Single Bitrate 16x9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md)
##### [H264 Single Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md)
##### [H264 Single Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md)
##### [H264 Single Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Single Bitrate 4x3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md)
##### [H264 Single Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md)
##### [H264 Single Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md)
##### [H264 Single Bitrate 720p for Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md)
##### [H264 Single Bitrate High Quality SD for Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md)
##### [H264 Single Bitrate Low Quality SD for Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md)
### Media Encoder Premium-munkafolyamat
#### [A Media Encoder Premium munkafolyamat formátumai és kodekei](media-services-premium-workflow-encoder-formats.md)
#### Kódolás a Media Encoder Premium munkafolyamat használatával
##### [Media Encoder Premium-munkafolyamat](media-services-encode-with-premium-workflow.md)
##### [Oktatóanyagok a Media Encoder Premium-munkafolyamathoz](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Speciális kódolási munkafolyamatok létrehozása a munkafolyamat-tervezővel](media-services-workflow-designer.md)
##### [Prémium munkafolyamat több bemenettel](media-services-media-encoder-premium-workflow-multiplefilesinput.md)
### [fMP4-adattömböket előállító feladat létrehozása](media-services-generate-fmp4-chunks.md)
### Médiafeldolgozók
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)
### [Hibakódok](media-services-encoding-error-codes.md)
### Elavult
#### [Statikus csomagolás és titkosítás](media-services-static-packaging.md)

## [Élő stream](media-services-manage-channels-overview.md)
### [helyszíni kódolók](media-services-live-streaming-with-onprem-encoders.md)
#### [Portal](media-services-portal-live-passthrough-get-started.md)
#### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
### [Élő stream a felhőalapú kódolóval](media-services-manage-live-encoder-enabled-channels.md)
#### [Azure Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
#### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
### [Helyszíni kódolók konfigurálása felhőalapú kódolóval való használatra](media-services-live-encoders-overview.md)
#### [Elemental Live kódoló](media-services-configure-elemental-live-encoder.md)
#### [FMLE kódoló](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster kódoló](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast kódoló](media-services-configure-wirecast-live-encoder.md)
### [A hosszú ideig futó műveletek kezelése](media-services-dotnet-long-operations.md)
### [Specifikáció: darabolt MP4 élő feldolgozása](media-services-fmp4-live-ingest-overview.md)

## [Védelem](media-services-content-protection-overview.md)
### [A tartalomvédelem konfigurálása az Azure Portalon](media-services-portal-protect-content.md)
### [AES-128 titkosítatlan kulcs konfigurálása a streamhez](media-services-protect-with-aes128.md)
### [A REST használata a tartalmak tárolási titkosításához](media-services-rest-storage-encryption.md)
### [A Media Services PlayReady licencsablon áttekintése](media-services-playready-license-template-overview.md)
### [A Widevine-licencsablon áttekintése](media-services-widevine-license-template-overview.md)
### [DRM-licenckézbesítés](media-services-deliver-keys-and-licenses.md)
### [Partnerek használata a Widevine-licencek Media Servicesbe való kézbesítéséhez](media-services-licenses-partner-integration.md)
#### [Az Axinom használata a Widevine-licencek Media Servicesbe való kézbesítéséhez  ](media-services-axinom-integration.md)
#### [A castLabs használata a Widevine-licencek Media Servicesbe való kézbesítéséhez](media-services-castlabs-integration.md)
### [A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata](media-services-protect-with-drm.md)
### [Az Apple FairPlay által védett HLS-tartalmak streamelése ](media-services-protect-hls-with-fairplay.md)
### [DRM-alrendszer vegyes kialakítása](hybrid-design-drm-sybsystem.md)
### [CENC többszörös DRM-mel és hozzáférés-vezérléssel](media-services-cenc-with-multidrm-access-control.md)
### Adategység-kézbesítési házirendek konfigurálása
#### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
#### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Tartalomkulcsok létrehozása
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Tartalomkulcs-engedélyezési házirend konfigurálása
#### [Azure Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)
### [AES-sel titkosított HLS lejátszása a Safariban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)
### [Hitelesítési jogkivonatok átadása](http://mingfeiy.com/how-client-pass-tokens-to-azure-media-services-key-delivery-services)

## [Elemzés](media-services-analytics-overview.md)
### [Média elemzése az Azure Portallal](media-services-portal-analyze.md)
### [Feldolgozás a 2. indexelővel](media-services-process-content-with-indexer2.md)
### [Feldolgozás az indexelővel](media-services-index-content.md)
#### [Előre beállított feladat](indexer-task-preset.md)
### [Feldolgozása Hyperlapse használatával](media-services-hyperlapse-content.md)
### [Feldolgozás arcérzékelővel](media-services-face-and-emotion-detection.md)
### [Feldolgozás mozgásérzékelővel](media-services-motion-detection.md)
### [Feldolgozás a Face Redactorral](media-services-face-redaction.md)
#### [Útmutatás a Face Redactorhoz](media-services-redactor-walkthrough.md)
### [Feldolgozás video-miniatűrökkel](media-services-video-summarization.md)
### [Feldolgozás OCR-rel](media-services-video-optical-character-recognition.md)

## [Telemetria konfigurálása](media-services-telemetry-overview.md)
###[.NET](media-services-dotnet-telemetry.md)
###[REST](media-services-rest-telemetry.md)

## Méretezés
### [Médiafeldolgozás](media-services-scale-media-processing-overview.md)
#### [Azure Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
### Streamvégpontok
#### [Azure Portal](media-services-portal-scale-streaming-endpoints.md)

## [Tartalom továbbítása](media-services-deliver-content-overview.md)
### [Dinamikus csomagolás](media-services-dynamic-packaging-overview.md)
### [Szűrők és dinamikus jegyzékek áttekintése](media-services-dynamic-manifest-overview.md)
#### [Szűrők létrehozása .NET használatával](media-services-dotnet-dynamic-manifest.md)
#### [Szűrők létrehozása REST használatával](media-services-rest-dynamic-manifest.md)
### [CDN gyorsítótárazási házirend a Media Services bővítményben](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### Tartalom közzététele
#### [Azure Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [Továbbítás letöltésen keresztül](media-services-deliver-asset-download.md)
### [Feladatátvételi streamelési forgatókönyv](media-services-implement-failover.md)

## Felhasználás
### [Médiatartalom lejátszása meglévő lejátszókkal](media-services-playback-content-with-existing-players.md)
### [Médiatartalom lejátszása a Media Playerrel](media-services-develop-video-players.md)
### Egyéb lejátszási beállítások
#### [Zökkenőmentes streamet biztosító Windows Áruházbeli alkalmazás](media-services-build-smooth-streaming-apps.md)
#### [HTML5-alkalmazás DASH.js-sel](media-services-embed-mpeg-dash-in-html5.md)
#### [Adobe Open Source Media Framework-lejátszók](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [Hirdetések ügyféloldali beillesztése](media-services-inserting-ads-on-client-side.md)
### [A Microsoft Smooth Streaming ügyfélportolási készlet licencelése](media-services-sspk.md)

## Integrálás
### [Az Azure Functions használata a Media Services szolgáltatással](media-services-dotnet-how-to-use-azure-functions.md)
### [Példák az Azure Functions használatára a Media Services szolgáltatásban](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

## Figyelés
### A feladat előrehaladásának ellenőrzése
#### [REST](media-services-rest-check-job-progress.md)
#### [Azure Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Feladatértesítések figyelése üzenetsor-tárolóval](media-services-dotnet-check-job-progress-with-queues.md)
### [Feladatértesítések figyelése webhookokkal](media-services-dotnet-check-job-progress-with-webhooks.md)

## Hibaelhárítás
### [Gyakori kérdések](media-services-frequently-asked-questions.md)
### [Hibaelhárítási útmutató az élő streameléshez](media-services-troubleshooting-live-streaming.md)
### [Hibakódok](media-services-error-codes.md)
### [Újrapróbálkozási logika](media-services-retry-logic-in-dotnet-sdk.md)

# Referencia
## [Kódminták](https://azure.microsoft.com/en-us/resources/samples/?service=media-services)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.media)
## [Azure PowerShell (Szolgáltatásfelügyelet)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.windowsazure.mediaservices.client)
## [REST](/rest/api/media/mediaservice)  

# Erőforrások
## [Azure Media Services-közösség](media-services-community.md)
## [Azure-ütemterv](https://azure.microsoft.com/roadmap/?category=web-mobile)
## [Díjszabás](https://azure.microsoft.com/pricing/details/media-services/)
## [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)
## [Kibocsátási megjegyzések](media-services-release-notes.md)
## [Videók](https://azure.microsoft.com/resources/videos/index/?services=media-services)
