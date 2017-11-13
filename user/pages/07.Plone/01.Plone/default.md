---
title: Plone
---

学内の某サイトのCMSに[Plone](http://www.plone.org/)を使ってますが，Plone 4.2からPlone 5.0にコンテンツを更新するのに随分苦労しました。

## Plone 4.2 から 5.0へのアップグレード手順

**問題点**:

1. Collageが廃止になった。
2. FacultyStaff が謎のエラーをだす。(もともと少しバギーだった)
3. Plone 4.x で多言語対応に使っていた LinguaPlone はPlone 5 では plone.app.multilingual に変更になっている。しかし，そのコンテンツ変換でエラーが出る。LinguaPloneを uninstall をしてもゴミが残るらしく，それで後々困る。
4. Quitagroup関連のアドオン(subskins等)の残骸がコンテンツに残っていて，アップグレードの邪魔をする。

**対策**
1. Collageを含むページは全部消す。(Collageのページが残っていると，Plone 5用にコンテンツ移行をする際にエラーで苦しむ)
2. FacultyStaffのページをそのままPlone5に移行するのは諦めて完全に消す。アップグレード後に手作業でつくり直す。
3. 英語ページもばっさり消して，アップグレード後に手作業でつくり直す(もともとコンテンツ少なかったし)。対応する英文ページへのリンクがある日本語ページも，そのリンク情報がPlone 5にしたときに悪さするので，リンク情報も消す。
4. subskinsにはお世話になったが，関連の設定は片っ端消す。
5. いろいろなアドオンの残骸を少しでも消せるよう，各コンテンツはZMI上でzexpにしてexport/importする

## アップグレード手順

多分，かなり回り道をしましたが，まとめるとこんなかんじでOKなはず。
無駄な手順も混じっているかもしれませんがあしからず。

### Plone 4.2 から 4.3へ


- サイト設定変更
    - テーマをsubskins からsunbirstにする
    - 不要なアドオンは片っ端無効にし，buildout.cfgからも消す
- バグのもとになるコンテンツ削除
    - Collageページ一覧を'ZMI=>uid_catalog=>Indexes=>portal_type'で確認して全削除
    - LinguaPlone対策
      - 日英相互リンクはずす
      - トップ直下にあるフォルダ"日本語(ja)"の中にあるコンテンツはすべてトップに移動し，フォルダ"日本語(ja)"は削除
      - 英語ページを削除(ZMI上でen削除実行)
    - FacultyStaffのページ削除
    - その他不要コンテンツ削除
- ZMI上で不要なアドオン関連の項目削除(以下を消した)
    - Attachment support
    - Ploneクラシックテーマ
    - Ploneboard
    - PloneFlashUpload
    - subskins
    - Collage
- `bin/buildout`
  - `buildout.cfg`から，Collage, FacultyStaff, LinguaPloneその他不要addonをはずす
  - `buildout.cfg`のPloneバージョン指定を4.3にする
  - `bin/buildout`
- `bin/plonectl`を実行して，コンテンツをPlone 4.3用に更新
- いろいろ掃除
  - サイト設定メニューでデータのパック(更新履歴は全部捨てる)
  - ZMI => portal_setup => manage
      - 以下の名前を含む項目があったら消す
      - Quintagroup PloneCaptchas import profile.
      - SubSkins
      - “invalid step handlers”
  - ZMI => portal_properties
      - 以下の名前を含む項目があったら消す    
      - => Contents の欄に “collage_properties”
      - => site_propertiesの欄に 'checkout_workflow_policy'
  - [情報源](https://community.plone.org/t/plone-migration-4-3-4-to-5-0-errors/1507/8)
  - ZMI => portal_controlpanel
      - “Collage entry”があったら消す
  - ZMI => portal_catalog => Advanced
      - “Clear and Rebuild”を選択 (Plone5にするときのエラー対策として重要)
  - ZMI => reference_catalog=>Advanced
      - "Update Catalog"を選択 (これも(多分)重要)
  - ZMI => uid_catalog => Advanced
      - “Update catalog”を選択 (これも(多分)重要)
  - コンテンツの出力
      - Plone 5に移動するためのコンテンツをZMI上で選択してexport。。コンテンはzexp形式で`zinstance/var/instance`以下に出力される。

### Plone 4.3上でデータのClean up

- 新規の空Plone4.3サイトを準備(build.cfgは最低限のアドオン追加)
  - ZMIで export portal_languages.zexp
  - 旧サイトで出力したzexpファイルを新規サイトの`zinstance/var/instance/import/`以下に移動。上で作成した`portal_languages.zexp`も同じところに移動
  - ZMI上のimport機能で，zexpファイルを読み込む
  - ZMI上でportal_languagesを消去して，上記で作成したzexpを読み込む
- 日本語フォルダ削除(中身はトップへ移動)
- LinguaPloneの痕跡削除
```
$ bin/instance debug
(Pdb) plone=app.Plone
(Pdb) site = plone.getSiteManager()
(Pdb) from plone.i18n.locales.interfaces import IContentLanguageAvailability
(Pdb) utils = site.getAllUtilitiesRegisteredFor(IContentLanguageAvailability)
(Pdb) utils
[<plone.i18n.locales.languages.ContentLanguageAvailability object at 0xb63c4cc>,
<ContentLanguages at /plone/plone_app_content_languages>,
<Products.LinguaPlone.vocabulary.SyncedLanguages object at 0xfa32e8c>,
<Products.LinguaPlone.vocabulary.SyncedLanguages object at 0xfa32eac>]
(Pdb) utils = utils[:-2]
(Pdb) del site.utilities._subscribers[0][IContentLanguageAvailability]
Repeat the procedure for IMetadataLanguageAvailability and commit the transaction:
(Pdb) import transaction
(Pdb) site._p_changed = True
(Pdb) site.utilities._p_changed = True
(Pdb) transaction.commit()
(Pdb) app._p_jar.sync()   # if zeo setup
```
- ZMIでおそうじ
  - ZMI => portal_catalog => Advanced
      - “Clear and Rebuild”を選択 <=　Plone5にするときのエラー対策として重要
  - ZMI => reference_catalog=>Advanced
      - "Update Catalog"を選択 <=これも(多分)重要
  - ZMI => uid_catalog => Advanced
      - “Update catalog”を選択 <=これも(多分)重要


### 4.3 => 5.0.7
- Plone5の新規サイトを準備
- 上記Plone 4.3の`zinstance/var`以下に有る`filestorage`と`blobstorage`をplone5サイトの同じ場所に移動(`cp -r`ではだめ。`mv`が一番安全。tarで固めてって言ってもいいが，Solarisのtarでは抜け落ちるファイルがあるらしく，コンテンツの移行時にエラーが出る)
- upgrades.pyのパッチ適用
  - dryrunで”ReferenceException … not referenceable…”とか，linkintegrity関係のエラーが出る場合の対応
  - [情報源](https://github.com/plone/plone.app.linkintegrity/issues/48)にあるように`<Plone 5 site>/buildout-cache/eggs/plone.app.linkintegrity-3.1-py2.7.egg/plone/app/linkintegrity/vupgrades.py` を修正(linkintegrityのバージョン名部分はちょっと違うかも)
  -  同じフォルダ内の`upgrades.pyo`と`upgrades.pyc`は削除。
- コンテンツをPlone5用に更新

### Plone5.0に移行したコンテンツのDexterity対応

Plone5用にコンテンツ更新がすんだら，Dexterityの形式に変換するか聞かれる。
幾つかのフォルダでエラーが出てとまった。
- 該当するフォルダを，Plone 4.3のサイトから削除してはZMIでのお掃除と，`var/{filestorage,blolstorage}`のPlone 5.0への移動を繰り返す。

Ploneのdebug機能で悪さをするコンテンツの洗い出しも可能。
記憶があいまいだが，多分以下の方法だったと思う。
```
$ sudo bin/instance debug
```
このあと，以下のコマンドを実行。
```
from Products.CMFCore.utils import getToolByName
site = app[‘Plone’]
zcatalog = getToolByName(site, 'portal_catalog')
cnt=0
brains = zcatalog.unrestrictedSearchResults()
for brain in brains:
  obj = brain._unrestrictedGetObject()
  cnt +=1
```
問題の有るオブジェクトがあったら，エラーメッセージを確認して以下を実行すると，そのオブジェクトのパスがわかる。
```
brains[cnt].getPath()
```
関連情報URL
- [Upgrade failed - Plone 4.3.8 to Plone 5.0.2
](https://stackoverflow.com/questions/36127714/upgrade-failed-plone-4-3-8-to-plone-5-0-2)
- [Manually Removing Local Persistent Utilities](https://docs.plone.org/manage/troubleshooting/manual-remove-utility.html)


## トラブル集

若干のトラブル対応集

### [トラブル]　Step languagetool has an invalid import handler

1. 空のploneをインストールして，ZMIで export portal_languages.zexp
2. 元のploneで，portal_languagesを消去して，上記zexpを読み込む

[情報源](https://stackoverflow.com/questions/33044956/upgrade-failed-plone-4-3-7-to-plone-5-0)

Thank you, that was the tip I needed. No 3rd party products are installed now, but perhaps I had tried out something years ago and some crud was left behind? In the ZMI, I created a new dummy Plone site, exported portal_languages to local disk, went back to my main Plone site, deleted portal_languages there, imported the file from disk, and THEN was able to continue with my Plone 4 -> Plone 5 migration and run the import tool successfully. – Jeff Shafer Oct 10 '15 at 16:14 

### [トラブル] buildout-cache以下の何かにpermission deniedと表示される

以下を実行する。
```
$ chmod -R g+r <Plone directory>/buildout-cache
$ chmod 777 <Plone directory>/zinstance
```

### [トラブル] AttributeError: 'NoneType' object has no attribute 'items' 

コンテンツのアップグレード時に出るエラー。

対策: http://plone.293351.n2.nabble.com/migration-from-plone-4-0-10-to-plone-4-1-3-fails-td7055584.html

### [トラブル] zexpのimportがうまくできない

```
Zope Error
Zope has encountered an error while publishing this resource.

Error Type: AttributeError
Error Value: reference_catalog
```
ZMI上で，portal_catalog, referene_catalog, uid_catalogの更新をする(上記アップグレード手順参照)と，なんとかなることが多い。

参考: [reference_catalog erro](http://plone-users.narkive.com/0O7HUeQp/reference-catalog-error)

### [トラブル] POSKeyError: ‘No blob file’ content in Plone

Plone コンテンツを新しいサイトに引っ越した時にこのエラーがでることがある。blobstorageの移動で抜け落ちたファイルやファイル情報があるのが原因。
このエラーが出たときに，ファイルは有るのに，ファイル名が無くなっていたりといったトラブルもあった。

対策は，コンテンツ移動に`cp -r`は使わないこと。tarで固めて持っていくのが一般的だが，下記サイトによるとSolarisのtarだと問題があるらしい。

参考:[Fixing POSKeyError: ‘No blob file’ content in Plone]( https://opensourcehacker.com/2012/01/05/fixing-poskeyerror-no-blob-file-content-in-plone/)