Linuxで使用される主なソフトウェアパッケージング形式（Deb, RPM, Flatpak, Snap, AppImage, ソースコード）のうち、とくにFlatpakとSnapについて、それぞれの特徴や違いをまとめました。

![](https://raw.githubusercontent.com/yKesamaru/flatpak_vs_snap/master/img/flatpak_vs_snap.png)

> Snap及びFlatpakの日本語入力については[日本語入力周りの整理・まとめ](https://zenn.dev/ykesamaru/articles/95ad7355c9bbba)をご参照ください。

https://zenn.dev/ykesamaru/articles/95ad7355c9bbba


# 主なパッケージング形式
## **DebとRPM**
DebianやUbuntu、Fedora、Red Hatなどで使用される。高速で効率的。依存関係の問題があるので、最新のアプリケーションは使用できない。自動アップデートあり。

## **Flatpak**
依存関係の問題を解決するために誕生。ランタイムが提供されるため、システムのライブラリに依存しない。しかし、セキュリティとディスクスペースの使用が問題。自動アップデートあり。

## **Snap**
Flatpakと似ていますが、サーバーサイドのソフトウェアも扱えます。デルタアップデート（アプリケーションやシステムが更新される際に、変更された部分だけをダウンロードして適用するタイプのアップデート方法）が可能。テーマのサポートが不足しており、起動が遅い場合があります。自動アップデートあり。

## **AppImage**
1つのパッケージにすべてが含まれているため、インストール不要。
アップデートが手動であり、テーマが統一されていません。自動アップデートなし。

## **ソースコード**
コンパイルが必要。自動アップデートなし。

# FlatpakとSnapの比較
## 特徴
### Flatpak
- **依存関係の解決**: 独自のランタイムを使用することで、システム全体のライブラリに依存しない設計。
- **セキュリティ**: アプリケーションはサンドボックス化されているが、脆弱なライブラリがランタイムに存在する可能性がある。
- **デスクトップオンリー**: その設計と目的がデスクトップアプリケーションに特化しているため、限定されたシステムリソースしかアクセスできず、サーバーサイドのソフトウェアは扱えない（扱いにくい）。
- **ディスクスペース**: 通常のDebやRPMよりも多くのディスクスペースを使用する。
- **リモートリポジトリ**: FlatHubなどのリモートからインストール可能。

### Snap
- **セキュリティ**: アプリケーションはサンドボックス化。ランタイム提供により、システム全体のライブラリに依存しない設計。すべてのパッケージはCanonicalによって一元管理されているSnap Storeから提供されるため、一定の監視はされている。
- **多機能性**: グラフィカルアプリケーションだけでなく、サーバーサイドのソフトウェアも扱える。
- **デルタアップデート**: 変更された部分のみをダウンロードしてアップデートする。
- **テーマサポート**: メインのデスクトップテーマをサポートしていない場合がある。
- **起動時間**: Flatpakや通常のパッケージよりも起動が遅い場合がある。
- **一元管理**: SnapはCanonicalによって管理されており、Snap Storeからのみインストール可能。

## 提供元
### Flathub

Flathubは、Flatpak用のソフトウェアデプロイメントとパッケージ管理ユーティリティのリポジトリ（Flatpak用語で言うと「リモートソース」）です。
[flathub.org](https://flathub.org/)
Flatpakでパッケージされたアプリケーションを取得するための事実上の標準となっています。
（Flathubとは独立したFlatpakリポジトリをホストすることも可能です。）

Flathubは、アプリケーションの開発者が直接ユーザーにアップデートを提供できるようにしています。
Snap StoreはCanonicalによって管理されており、一定レベルのセキュリティ監視が行われていますが、Flathubはコミュニティ駆動です。

### Snap Store
Snap Storeは、Snapパッケージを配布するためのオンラインプラットフォームで、Canonicalによって運営・メンテナンスされています。

Ubuntuの場合、Snapはデフォルトでインストールされています。以下は、Snap Storeからアプリをインストールするコマンドです。

```bash
# アプリをインストールするコマンド
sudo snap install [アプリ名]
```

### FlathubとSnap Storeの違い
1. **運営体**: Snap StoreはCanonicalによって、Flathubはコミュニティによって運営されています。
2. **パッケージ形式**: Snap StoreはSnap形式、FlathubはFlatpak形式です。
3. **依存関係**: 両者ともにランタイムを使用して依存関係を解決しています。Snapでは、このようなランタイム環境を「base snap」と呼びます。base snapは、アプリケーションが依存する基本的なライブラリやコンポーネントを含んでいます。Flatpakでも同様に、ランタイムがアプリケーションの依存関係を管理します。Flatpakのランタイムは、アプリケーションが必要とするライブラリや環境変数、ファイルシステムのレイアウトなどを提供します。
4. **更新**: Snapは自動更新、Flatpakは手動または自動（設定による）です。


# FlatpakとSnapの共通のメリット

1. **依存関係の管理**: 両方ともアプリケーションとその依存関係を一緒にパッケージングするため、依存関係の問題を大幅に減らします。

2. **セキュリティ**: アプリケーションは独立した環境（サンドボックス）で実行されるため、他のシステムコンポーネントと隔離され、セキュリティが向上します。

3. **ディストリビューション間の互換性**: 両方とも多くのLinuxディストリビューションで動作する設計になっているため、異なるディストリビューション間でのアプリケーションの移行が容易です。

4. **アップデートとロールバック**: アプリケーションのアップデートや、必要な場合にはバージョンのロールバックが容易に行えます。

5. **アプリケーションの豊富な選択肢**: それぞれが独自の大規模なリポジトリ（Flathub、Snap Store）を持っているため、多くのアプリケーションが簡単にインストールできます。

6. **自己完結型**: 両方ともアプリケーションが必要とするほぼすべてのファイルを含んでいるため、他のソフトウェアとの競合が少ないです。

7. **メンテナンスの容易性**: アプリケーションとその依存関係が一緒に管理されるため、システムのメンテナンスが容易です。



# FlatpakとSnapの共通のデメリット

1. **ディスク使用量**: 両方ともアプリケーションとその依存関係を一緒にパッケージングするため、ディスク使用量が増加する可能性があります。

2. **起動時間**: 一部のアプリケーションでは、FlatpakやSnapの形式でインストールした場合、起動時間が長くなることがあります。

3. **テーマとの整合性**: システムのテーマやアイコンが必ずしも適用されない場合があります。

4. **パフォーマンス**: サンドボックス化された環境で動作するため、ネイティブなパッケージに比べてわずかにパフォーマンスが低下する可能性があります。

5. **複雑性**: 既存のパッケージ管理システムとは異なるコマンドや操作が必要な場合があり、学習曲線が存在します。

6. **セキュリティの二面性**: サンドボックス化はセキュリティを高めますが、アプリケーションが古い依存関係を持っていると、それがセキュリティリスクになる可能性があります。

7. **リポジトリの制限**: FlathubやSnap Storeなど、特定のリポジトリからしかアプリケーションをインストールできない制限があります。

# ランタイム
ランタイム（Runtime）とは、一連の共有ライブラリや依存関係をまとめたものです。これにより、複数のFlatpak/snapアプリケーションが同じ基本的なライブラリやリソースを共有できます。ランタイムは、アプリケーションが実行される際の「環境」を提供する役割を果たします。

## ランタイムが必要な理由

1. **依存関係の簡素化**: ランタイムを使用することで、各アプリケーションが個別に依存関係を持つ必要がなくなります。これにより、ディスク使用量が削減される場合があります。

2. **互換性**: ランタイムが提供する共通のライブラリと環境により、アプリケーションはさまざまなLinuxディストリビューションで互換性を持つようになります。

3. **セキュリティとメンテナンス**: ランタイム自体が更新されると、それを使用するすべてのアプリケーションも自動的にその更新を受けることができます。これにより、セキュリティパッチやバグ修正が効率的に行えます。

4. **開発の効率性**: 開発者は、ランタイムが提供する共通のライブラリやツールを使用してアプリケーションを開発できるため、開発プロセスが効率化されます。

## ランタイムのメンテナンス
### Flatpak
ランタイムは主にFlathubから提供されますが、Flathub自体はセキュリティ監視のプロセスについて明確に公表していません。
一般的には、ランタイムを提供する各プロジェクトやコミュニティがセキュリティの監視を行っています。
### Snap
ランタイム（Snapcraftでいう「base snap」や「core snap」）はCanonicalによって管理されています。Canonicalはセキュリティ監視を行い、必要な場合はセキュリティアップデートを提供します。

どちらの場合も、ランタイムが使用する各ライブラリやコンポーネントはオープンソースであり、セキュリティ研究者やコミュニティによっても監視されています。

## ランタイムに必要なライブラリがない場合
SnapやFlatpakでランタイムに含まれないライブラリが必要な場合、アプリケーション開発者はいくつかの方法で対処できます。

### Snap:

1. **バンドル**: アプリケーションと一緒に必要なライブラリをバンドル（同梱）できます。
2. **ステージング**: `snapcraft.yaml` ファイル内で、ステージングエリアに必要なライブラリをコピーする指示を追加できます。
3. **インターフェイスとプラグイン**: カスタムインターフェイスやプラグインを使用して、システムのリソースにアクセスすることもあります。

### Flatpak:

1. **バンドル**: 必要なライブラリをFlatpakアプリケーションと一緒にバンドルできます。
2. **拡張**: Flatpakでは、ランタイムを拡張する形で追加のライブラリを提供できます。
3. **ビルドマニフェスト**: `flatpak-builder` のビルドマニフェスト（通常はJSONまたはYAMLファイル）に、追加のライブラリに関するビルド指示を追加できます。

これらの方法により、アプリケーションはランタイムに含まれない依存関係も解決できます。バンドルされたライブラリが古くなった場合には、アプリケーション開発者がそれを更新する責任があり、セキュリティリスクをもたらす可能性があります。

# 雑感
FlatpakもしくはSnapの両方において、どちらかのパッケージ形式しか提供されていない、もしくはどちらかのパッケージ形式にバグがある、というシーンに出会ったことがあります。複数のパッケージ形式が存在していることが、良くも悪くも影響しているのでしょう。

Canonicalが具体的にどのような思想をもっているか分かりませんが、Snapを強く推し、Flatpakを導入しないことについて、色々な意見があります。個人的な感想ですが、Flatpakのシステムでは、セキュリティの責任がとれないからでしょう。Snapを一箇所で管理していることも、そこに原因があると思います。

Flatpak/Snapどちらでも良いのですが、処理速度が問われるようなブラウザなどの一部アプリケーションは、deb形式を残しておいて欲しいと思います。