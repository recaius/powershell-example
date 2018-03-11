トークンの取得
----

```powershell
$json = @{ machine_translation= @{ service_id= ''; password= '' } } | ConvertTo-Json
$token = Invoke-RestMethod https://api.recaius.jp/auth/v2/tokens -Method Post -Body $json -ContentType 'application/json'
```

お使いのサービスIDとパスワードを設定してください。

取得されたトークンは `$token.token` に格納されます。


口語翻訳
----

```powershell
$json = @{ mode= 'spoken_language'; query= '本日は晴天なり'; src_lang='ja'; tgt_lang= 'en'; arrange= 'true' } | ConvertTo-Json
$jsonBytes = [System.Text.Encoding]::UTf8.GetBytes($json)
$headers = @{ 'X-Token'= $token.token }
$trans = invoke-restmethod https://api.recaius.jp/mt/v2/translate -Method Post -Body $jsonBytes -ContentType 'application/json' -Headers $headers
```

翻訳したい文字列は、UTF8のバイト列とする必要があります。

翻訳されたテキストは `$trans.data.translations[0].translatedText` に格納されます。


トークンの破棄
----

```powershell
$headers = @{ 'X-Token'= $token.token }
invoke-restmethod https://api.recaius.jp/auth/v2/tokens -Method Delete -ContentType 'application/json' -Headers $headers
```
