---
title: "権利の使用"
description: "権利は特殊なアプリの機能およびセキュリティのアクセス許可であり、これらを使用するように正しく構成されたアプリケーションに付与されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: b470eb7450b18c25e1a6aa58095dce90f36cbe8e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-entitlements"></a>権利の使用

_権利は特殊なアプリの機能およびセキュリティのアクセス許可であり、これらを使用するように正しく構成されたアプリケーションに付与されます。_

iOS では、アプリは_サンドボックス_で実行されます。このサンドボックスでは、アプリケーションと特定のシステム リソースまたはユーザー データ間のアクセスを制限する一連の規則が提供されます。 _権利_は、アプリに追加機能を提供するためにシステムでサンドボックスを拡張することを要求する場合に使用されます。

アプリの機能を拡張するには、アプリの Entitlements.plist ファイルで権利を指定する必要があります。 特定の機能のみを拡張することができ、これらの機能については、「[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)」ガイドにリストされており、[以下](#keyreference)で説明します。 権利はキー/値ペアとしてシステムに渡され、通常は機能ごとに 1 つだけ必要になります。 特定のキーと値については、このガイドの後述の「[権利キー参照](#keyreference)」で説明します。
Visual Studio for Mac と Visual Studio では、Entitlements.plist エディターを介して Xamarin.iOS アプリで権利を追加するためのわかりやすいインターフェイスが提供されます。
このガイドでは、Entitlements.plist エディターおよびその使用方法を紹介します。 また、機能ごとに iOS プロジェクトに追加可能なすべての権利の参照も提供します。

## <a name="entitlements-and-provisioning"></a>権利とプロビジョニング


Entitlements.plist ファイルは権利の指定と、アプリケーション バンドルへの署名の際に使用されます。

ただし、アプリが正しくコード署名されていることを確認するには、追加のプロビジョニングがいくつか必要になります。 使用するプロビジョニング プロファイルに、必要な機能が有効になっているアプリ ID を含める必要があります。 これを行う方法については、「[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)」ガイドを参照してください。

> [!IMPORTANT]
> 注: Entitlements.plist ファイルは、機能を使用してアプリケーションに適切なプロパティを入力する際に役立ちますが、Apple 開発者アカウントにリンクされていないため、プロビジョニング プロファイルを生成することはできません。 そのため、アプリケーションを展開して配布するには、開発者ポータルを使用してプロビジョニング プロファイルを生成する必要があります。

## <a name="set-entitlements-in-a-xamarinios-project"></a>Xamarin.iOS プロジェクトで権利を設定する

アプリ ID を定義する際に必要なアプリケーション サービスを選択して構成するだけでなく、権利を Xamarin.iOS プロジェクトで構成する必要もあります。その場合は、**Info.plist** ファイルと **Entitlements.plist** ファイルを編集します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac で権利を構成するには、次の操作を行います。

1. **ソリューション エクスプローラー**で、**Info.plist** ファイルをダブルクリックして開き、編集します。
2. **[iOS アプリケーションのターゲット]** セクションで、アプリケーションの名前を入力し、アプリ ID が定義されたときに作成された**バンドル ID** を入力します。

  ![](entitlements-images/servicexs01.png "バンドル ID を入力します")

3. 変更内容を **Info.plist** ファイルに保存します。
4. **ソリューション エクスプローラー**で、**Entitlements.plist** ファイルをダブルクリックして開き、編集します。

    ![](entitlements-images/servicexs02.png "権利の編集")

5. Xamarin.iOS アプリケーションに必要な権利を選択して構成し、アプリ ID が作成されたときに定義された設定と一致するようにします。
6. 変更内容を **Entitlements.plist** ファイルに保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio で権利を構成するには、次の操作を行います。

1. **ソリューション エクスプローラー**で、**Info.plist** を右クリックし、**[プログラムから開く]** を選択し、 **[プロパティ一覧エディター]** ファイルを選択して開き、編集します。
2. **[iOS アプリケーションのターゲット]** セクションで、アプリケーションの名前を入力し、アプリ ID が定義されたときに作成された**バンドル ID** を入力します。

  ![](entitlements-images/servicevs01.png "バンドル ID の設定")

3. 変更内容を **Info.plist** ファイルに保存します。
4. **ソリューション エクスプローラー**で、**Entitlements.plist** ファイルを右クリックし、**[プログラムから開く]** を選択し、 **[プロパティ一覧エディター]** を選択して開き、編集します。

    ![](entitlements-images/servicevs02.png "権利の編集")

    または、**Entitlements.plist** ファイルをダブルクリックして XML ソース エディターを開きます。このエディターでは、後述の[権利の参照](#keyreference)に関するセクションで詳しく説明されているように、権利プロパティとキーの値を設定することができます。

5. Xamarin.iOS アプリケーションに必要な権利を選択して構成し、アプリ ID が作成されたときに定義された設定と一致するようにします。
6. 変更内容を **Entitlements.plist** ファイルに保存します。


-----

<a name="add-new" />

## <a name="adding-a-new-entitlementsplist-file"></a>新しい Entitlements.plist ファイルの追加

権利は、Entitlements.plist ファイルを介してアプリに追加されます。 このファイルは、既定で Xamarin.iOS プロジェクトに含まれますが、古いプロジェクトには存在しない可能性があります。

Xamarin.iOS に Entitlements.plist ファイルを追加するには、次の操作を行います。

1.  プロジェクト ファイルを右クリックし、**[追加]、[新しいファイル...]** の順に参照します。

    ![[ファイルの追加] コンテキスト メニュー](entitlements-images/image1.png)
2.  [新しいファイル] ダイアログで、**[iOS]、[プロパティ一覧]** の順に選択し、Entitlements という名前を付けます。

    ![[新しいファイル] ダイアログ](entitlements-images/image2.png)

<a name="keyreference" />

## <a name="entitlement-key-reference"></a>権利キー参照

Entitlements.plist エディターの [ソース] パネルを使用して、権利キーを追加することができます。 必要なキーは通常、Entitlements.plist エディターを使用する際に追加されますが、参考までに以下にリストします。

### <a name="wallet"></a>ウォレット

*   **説明**: 正式には Passbook として知られるウォレットは、パスを格納して管理するアプリです。 これらのパスには、クレジット カード、ストア カード、搭乗券、チケットなどがあります。

    - **パスの種類の識別子**
        * **キー**: com.apple.developer.pass-type-identifiers
        * **文字列**: `$(TeamIdentifierPrefix)*`

* **注**:
    - これにより、アプリはすべてのパスの種類を許可できるようになります。 アプリを制限し、チームのパスの種類のサブセットのみを許可するには、文字列値を `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)` に設定します。

    この pass.$(CFBundleIdentifier) は、[ここ](~/ios/platform/passkit.md)で作成されたパス ID です。

<a name="icloud" />

### <a name="icloud"></a>iCloud

*   **説明**: iCloud は、iOS ユーザーに自分のコンテンツを格納し、デバイス間で共有するための便利で簡単な方法を提供します。 開発者が iCloud を使用して、ユーザーにストレージの手段を提供できる方法は 4 つあります。つまり、Key-Value ストレージ、UIDocument ストレージ、CoreData、および CloudKit を直接使用して個別のファイルとディレクトリにストレージを提供できます。 これらの詳細については、iCloud の概要に関するガイドを参照してください。

    - **iCloud ドキュメントと CloudKit**
        - **キー**: com.apple.developer.ubiquity-container-identifiers
        - **文字列**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **iCloud KeyValue ストレージ**
        - **キー**: com.apple.developer.ubiquity-kvstore-identifier
        - **文字列**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **注**:
    - `$(TeamIdentifierPrefix)` 文字列は、developer.apple.com にログインし、**[メンバー センター]、[あなたのアカウント]、[開発者アカウントの概要]** の順に移動して、チーム ID (または開発者の個別の ID) を取得することで確認できます。 これは 10 桁の文字列 (A93A5CM278 など) になります。
    - `$(CFBundleIdentifier)` 文字列は `iCloud` で始まり、iCloud コンテナーは「[機能の使用](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)」ガイドの手順に従って作成されるときに設定されます。
    - $`(TeamIdentifierPrefix)` および `$(CFBundleIdentifier)` プレースホルダーを使用することができ、ビルド時に適切な値に置き換えられます。

### <a name="app-groups"></a>アプリ グループ

- **説明**: アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。

    - **キー**: com.apple.security.application-groups
    - **文字列**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Apple Pay

- **説明**: Apple Pay を使用すると、ユーザーは自分の iOS デバイスから物理的な商品の支払いができます。
    - **キー**: com.apple.developer.in-app-payments
    - **文字列**: merchant.your.mechantid

### <a name="push-notifications"></a>プッシュ通知

- **キー**: aps-environment
- **文字列**: `production` または `development`

### <a name="siri"></a>Siri

- **説明**: SiriKit を使用すると、iOS アプリは、アプリ拡張機能と新しい Intents および Intents UI フレームワークを使用して、iOS デバイス上のマップ アプリと Siri にアクセス可能なサービスを提供できます。 詳細については、SiriKit の概要に関するガイドを参照してください。
    - **キー**: com.apple.developer.siri

### <a name="personal-vpn"></a>個人の VPN

- **キー**: com.apple.developer.networking.vpn.api
- **文字列**: allow-vpn

### <a name="keychain-sharing"></a>キーチェーンの共有

- **説明**: キーチェーンの共有を使用すると、アプリ開発者は、デバイス キーチェーンに格納されているパスワードを、同じチームによって開発された他のアプリと共有できます。 文字列にキーチェーン アクセス グループ識別子を渡すことで、アクセスを制限することができます。
    - **キー**: keychain-access-groups
    - **文字列**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Inter-App オーディオ

- **説明**: Inter-App オーディオを使用すると、開発者はアプリ間でオーディオをストリーミングできます。
    - **キー**: inter-app-audio
    - **ブール値**: YES

### <a name="associated-domains"></a>関連付けられているドメイン

- **説明**: ユニバーサル リンクとして処理する必要がある関連付けられているドメインは、この権利と共に渡す必要があります。 ユニバーサル リンクを実装することで、アプリと Web サイト間にディープ リンクを設定できます。 アプリでサポートされる各ドメインにエントリを指定する必要があり、各エントリは `applinks:` で始まる必要があります。
    - **キー**: com.apple.developer.associated-domains
    - **文字列**: webcredentials:example.com

### <a name="data-protection"></a>データの保護

- **説明**: データの保護を有効にする場合は、組み込みの暗号化ハードウェアを使用して、暗号化された形式でアプリで使用される機密データを格納します。 既定では、保護レベルは完全な保護に設定されます (デバイスのロックが解除されたときにのみ、ファイルにアクセスできます)。
    - **キー**: com.apple.developer.default-data-protection
    - **文字列**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **説明**: HomeKit フレームワークでは、サポートされているホーム オートメーション デバイス (iOS デバイスのものすべて) を設定、構成、および管理するためのプラットフォームが提供されます。 HomeKit の使用の詳細については、HomeKit の概要に関するガイドを参照してください。
    - **キー**: com.apple.developer.homekit
    - **ブール値**: YES

### <a name="healthkit"></a>HealthKit

- **説明**: HealthKit は iOS 8 で導入されたフレームワークです。正常性に関する情報のための集中型の調整されたセキュア データ ストアを提供します。 HealthKit の使用の詳細については、HealthKit の概要に関するガイドを参照してください。
    - **キー**: com.apple.developer.healthkit
    - **ブール値**: YES

### <a name="wireless-accessory-configuration"></a>Wireless Accessory Configuration

- **説明**: Wireless Accessory Configuration を使用すると、アプリケーションで MFi Wi-Fi アクセサリを構成できます。
    - **キー**: com.apple.external-accessory.wireless-configuration
    - **ブール値**: YES

## <a name="summary"></a>まとめ

このガイドでは、権利について説明し、Visual Studio for Mac と Visual Studio での権利の使用方法について紹介しました。 また、機能ごとにキー/値ペアの参照も提供しました。