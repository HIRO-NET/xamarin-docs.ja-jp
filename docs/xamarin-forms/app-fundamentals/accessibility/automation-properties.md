---
title: オートメーションのプロパティ
description: この記事では、スクリーン リーダーでページの要素について読み上げることができるように、Xamarin.Forms アプリケーションで AutomationProperties クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: f59a528916d2cc5efd19ba7c35a7b4f041ecbe09
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056163"
---
# <a name="automation-properties-in-xamarinforms"></a>Xamarin.Forms でのオートメーション プロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)

_Xamarin.Forms では、AutomationProperties クラスの添付プロパティを使用して、ユーザー インターフェイス要素にアクセシビリティの値を設定できます。それによって、ネイティブなアクセシビリティ値が設定されます。この記事では、スクリーン リーダーでページの要素について読み上げることができるように、AutomationProperties クラスを使用する方法について説明します。_

Xamarin.Forms では、次の添付プロパティを使用して、ユーザー インターフェイス要素にオートメーション プロパティを設定できます。

- `AutomationProperties.IsInAccessibleTree` – 要素がアクセシビリティの高いアプリケーションで使用できるかどうかを示します。 詳しくは、「[AutomationProperties.IsInAccessibleTree](#isinaccessibletree)」をご覧ください。
- `AutomationProperties.Name` – 要素の読み上げ可能な識別子として機能する、要素の簡単な説明です。 詳しくは、「[AutomationProperties.Name](#name)」をご覧ください。
- `AutomationProperties.HelpText` – 要素の詳しい説明です。要素に関連付けられているヒント テキストと考えることができます。 詳しくは、「[AutomationProperties.HelpText](#helptext)」をご覧ください。
- `AutomationProperties.LabeledBy` – 現在の要素に対するアクセシビリティ情報を、別の要素で定義できます。 詳しくは、「[AutomationProperties.LabeledBy](#labeledby)」をご覧ください。

これらの添付プロパティでは、スクリーン リーダーが要素について読み上げることができるように、ネイティブのアクセシビリティ値が設定されます。 添付プロパティについて詳しくは、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」をご覧ください。

> [!IMPORTANT]
> `AutomationProperties` の添付プロパティを使用すると、Android での UI テストの実行に影響を与える可能性があります。 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) と、`AutomationProperties.Name` および `AutomationProperties.HelpText` プロパティの両方で、ネイティブな `ContentDescription` プロパティが設定され、`AutomationProperties.Name` および `AutomationProperties.HelpText` プロパティの値の方が `AutomationId` の値より優先されます (`AutomationProperties.Name` と `AutomationProperties.HelpText` が両方とも設定されている場合は、値が連結されます)。 つまり、要素で `AutomationProperties.Name` または `AutomationProperties.HelpText` も設定されている場合、`AutomationId` を検索するテストは失敗ます。 このシナリオでは、代わりに `AutomationProperties.Name` または `AutomationProperties.HelpText`、あるいは両方を連結したものを検索するように、UI テストを変更する必要があります。

アクセシビリティ値を読み上げるスクリーン リーダーは、プラットフォームごとに異なります。

- iOS では VoiceOver が使用されます。 詳しくは、developer.apple.com の「[Test Accessibility on Your Device with VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)」(VoiceOver を使用するデバイスでのアクセシビリティをテストする) をご覧ください。
- Android では TalkBack が使用されます。 詳しくは、「[Testing Your App's Accessibility](https://developer.android.com/training/accessibility/testing.html#talkback)」(アプリのアクセシビリティのテスト) をご覧ください。
- Windows ではナレーターが使用されます。 詳しくは、「[ナレーターを使用してメイン アプリのシナリオを検証する](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/)」をご覧ください。

ただし、スクリーン リーダーの厳密な動作は、ソフトウェアおよびそのユーザー構成に依存します。 たとえば、ほとんどのスクリーン リーダーでは、フォーカスを受け取ったコントロールに関連付けられているテキストが読み上げられて、ユーザーはページ上のコントロール間を移動しながら自分がいる場所を知ることができます。 一部のスクリーン リーダーでは、ページが表示されたときにアプリケーションのユーザー インターフェイス全体も読み上げられ、ユーザーはページで使用可能なすべての情報コンテンツを受け取ってから、ナビゲートを試みることができます。

また、スクリーン リーダーで読み上げられるアクセシビリティ値も異なります。 サンプル アプリケーションでは次のようになります。

- VoiceOver では、`Entry` の `Placeholder` の値、コントロールの使用方法の順に読み上げられます。
- TalkBack では、`Entry` の `Placeholder` の値、`AutomationProperties.HelpText` の値、コントロールの使用方法の順に読み上げられます。
- ナレーターでは、`Entry` の `AutomationProperties.LabeledBy` の値、コントロールの使用方法の順に読み上げられます。

さらに、ナレーターでは、`AutomationProperties.Name`、`AutomationProperties.LabeledBy`、`AutomationProperties.HelpText` の優先順位になります。 Android の TalkBack では、`AutomationProperties.Name` と `AutomationProperties.HelpText` の値が結合できます。 そのため、各プラットフォームでアクセシビリティのテストを十分に行い、最適なエクスペリエンスを確認することをお勧めします。

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` 添付プロパティは `boolean` であり、要素にアクセシビリティがあるかどうか、したがってスクリーン リーダーで認識できるかどうかが決定されます。 他のアクセシビリティ添付プロパティを使用するには、これが `true` に設定されている必要があります。 XAML では次のようにしてこれを実現できます。

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

または、C# で次のようにして設定できます。

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) メソッドを使用して `AutomationProperties.IsInAccessibleTree` 添付プロパティを設定することもできることに注意してください。`entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` 添付プロパティの値は、スクリーン リーダーで要素の読み上げに使用される簡潔でわかりやすいテキスト文字列である必要があります。 ユーザー インターフェイスの内容の理解や操作に重要な意味を持つ要素に対しては、このプロパティを設定する必要があります。 XAML では次のようにしてこれを実現できます。

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

または、C# で次のようにして設定できます。

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) メソッドを使用して `AutomationProperties.Name` 添付プロパティを設定することもできることに注意してください。`activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` 添付プロパティにはユーザー インターフェイス要素を説明するテキストを設定する必要があり、要素に関連付けられたヒント テキストと考えることができます。 XAML では次のようにしてこれを実現できます。

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

または、C# で次のようにして設定できます。

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) メソッドを使用して `AutomationProperties.HelpText` 添付プロパティを設定することもできることに注意してください。`button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

一部のプラットフォームの [`Entry`](xref:Xamarin.Forms.Entry) などの編集コントロールでは、`HelpText` プロパティを省略し、プレースホルダー テキストに置き換えることができる場合があります。 たとえば、"ここに名前を入力します" などは、ユーザーが実際に入力する前にコントロールにテキストを配置する [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) プロパティに適した候補です。

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` 添付プロパティを使用すると、現在の要素に対するアクセシビリティ情報を、別の要素で定義できます。 たとえば、[`Entry`](xref:Xamarin.Forms.Entry) の隣の [`Label`](xref:Xamarin.Forms.Label) を使用して、`Entry` の内容を説明できます。 XAML では次のようにしてこれを実現できます。

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

または、C# で次のようにして設定できます。

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) メソッドを使用して `AutomationProperties.IsInAccessibleTree` 添付プロパティを設定することもできることに注意してください。`entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="related-links"></a>関連リンク

- [関連付けられたプロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [アクセシビリティ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
