## はじめに

*(Mac環境の記事ですが、Windows環境も同じ手順になります。環境依存の部分は読み替えてお試しください。)*

この記事を最後まで読むと、次のことができるようになります。

- データの引き渡しなど複雑な処理の実装方法について理解する
- JavaScriptを使って実装する

`イメージ画像`

| Input                                                                                                                                   |     | Output                                                                                                                                  |
| :-------------------------------------------------------------------------------------------------------------------------------------: | :-: | :-------------------------------------------------------------------------------------------------------------------------------------: |
| <img width="200" alt="IMG_0438.PNG" src="https://qiita-image-store.s3.amazonaws.com/0/326996/c2b3018d-239e-dfa3-a4c5-e8259d970e7c.png"> | >>> | <img width="200" alt="IMG_0439.PNG" src="https://qiita-image-store.s3.amazonaws.com/0/326996/bb8147cf-e805-965a-8e6d-cd69e1f0698d.png"> |

```countUp.js
countUp: function(btn, context) {
    var rowIndex = context.rowIndex;
    var sectionIndex = context.sectionIndex;
    var rowData = context.widgetInfo.data[rowIndex];

    if (Number(rowData.txtCounter.text) + 1 > 20) {
        return;
    }
    rowData.txtCounter.text = Number(rowData.txtCounter.text) + 1;

    context.widgetInfo.setDataAt(rowData, rowIndex, sectionIndex);
}
```

## 関連する記事

- [Kony AppPlatformを使ってiOS/Androidアプリを作成する](https://qiita.com/nsuhara/items/c28d838492512850520c)
- [Kony AppPlatformで作成したiOS/AndroidアプリのAuto Layoutについて学ぶ](https://qiita.com/nsuhara/items/a52abd9861c51823ecec)
- [Kony AppPlatformで作成したiOS/AndroidアプリとSalesforceをデータ連携する](https://qiita.com/nsuhara/items/756120f1bddc6f8fe78b)

## 実行環境

|       環境      |   Ver.  |
|-----------------|---------|
| macOS Mojave    | 10.14.1 |
| Kony Visualizer | 8.3.10  |

## ソースコード

実際に実装内容やソースコードを追いながら読むとより理解が深まるかと思います。是非ご活用ください。

[GitHub](https://github.com/nsuhara/kony-Coding.git)

## Kony AppPlatformの特徴

簡単な処理はプログラミングレスで開発できますが、複雑な処理はコーディングによる実装が必要となります。クロスプラットフォームを採用しているため、iOSアプリ(Swift)やAndroidアプリ(Java)のプログラミング言語を使用せず`JavaScript`で実装します。また、Swiftの内部構成と同様に1つの画面(Form)に対して1つのControllerが自動で割り当てられます。

## コーディングの方法

画面(Form)を作成すると`Controller`と`ControllerActions`が自動生成されます。コードを書く際はこの2つのControllerに実装します。

|      自動生成     |                                                              説明                                                              |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Controller        | 汎用的な処理はControllerにコーディングします。作成したFunctionはActionの*Invoke Function*から呼び出して使用します。            |
| ControllerActions | Actionの*Add Snippet*などでコーディングしたものが自動的に追加されます。ControllerActionsに直接コーディングする事はできません。 |

<img width="200" alt="スクリーンショット 2018-12-23 0.18.58.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/fb21700c-ccd9-fd9e-ff34-18a5de10c54c.png">

## 実装サンプル

- ボタンを押してテキストボックスの値をカウントアップする

|   項目名   |   オブジェクト   |      説明      |
|------------|------------------|----------------|
| lblTitle   | ラベル           | タイトル       |
| txtCounter | テキストボックス | カウント表示   |
| btnCountUp | ボタン           | カウントアップ |

`画面イメージ`

<img width="500" alt="スクリーンショット 2018-12-23 22.06.05.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/45701d1a-3e0a-bfc4-7f06-c9c5441f6681.png">

### Controllerに実装するケース

1. Controller画面を作成します。Form名は`frmController`とします。

1. Projectタブ -> Mobile -> Controllers -> `frmControllerController`をクリックして`define`の中にカウントアップのコードを書きます。

    ```frmControllerController.countUp.js
    countUp: function() {
        this.view.txtCounter.text = Number(this.view.txtCounter.text) + 1;
    }
    ```

1. Projectタブ -> Mobile -> Forms -> frmController -> `btnCountUp`をクリックします。次にPropertiesタブ -> Actionたタブの`onClick`の`Edit`をクリックします。

    <img width="300" alt="スクリーンショット 2018-12-23 21.40.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/35acd232-98be-7e47-365a-cf5db506e425.png">

1. Functionsの中から`Invoke Function`をクリックします。次にFunction Nameから`countUp`を設定します。

    <img width="500" alt="スクリーンショット 2018-12-23 22.03.39.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/0551a7d0-539a-b372-d43c-6b5f03a9147e.png">

### ControllerActionsに実装するケース

1. ControllerActions画面を作成します。Form名は`frmControllerActions`とします。

1. Projectタブ -> Mobile -> Forms -> frmControllerActions -> `btnCountUp`をクリックします。次にPropertiesタブ -> Actionたタブの`onClick`の`Edit`をクリックします。

    <img width="300" alt="スクリーンショット 2018-12-23 21.40.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/35acd232-98be-7e47-365a-cf5db506e425.png">

1. Functionsの中から`Add Snippet`をクリックします。次にカウントアップのコードを書きます。

    ```frmControllerActionsControllerActions.js
    self.view.txtCounter.text = Number(this.view.txtCounter.text) + 1;
    ```

    <img width="500" alt="スクリーンショット 2018-12-23 22.24.31.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/b07d01fe-5ce4-11f6-045a-8b1f80213da5.png">

1. Projectタブ -> Mobile -> Controllers -> `frmControllerActionsControllerActions`をクリックして`define`の中にSnippetのコードが自動で追加されたことを確認します。

    <img width="500" alt="スクリーンショット 2018-12-23 22.19.25.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/cc563836-038f-7806-7652-2258fc788560.png">

## 複雑な処理の実装

### シナリオ

- アプリは入力画面と出力画面の2つで構成する
- 入力画面はリスト一覧を持ち1件ごとに個数のカウントアップ/ダウンを可能とする
- 出力画面は入力データを引き継ぎデータを結合して表示する

`画面イメージ`

| Input                                                                                                                                   |     | Output                                                                                                                                  |
| :-------------------------------------------------------------------------------------------------------------------------------------: | :-: | :-------------------------------------------------------------------------------------------------------------------------------------: |
| <img width="200" alt="IMG_0438.PNG" src="https://qiita-image-store.s3.amazonaws.com/0/326996/c2b3018d-239e-dfa3-a4c5-e8259d970e7c.png"> | >>> | <img width="200" alt="IMG_0439.PNG" src="https://qiita-image-store.s3.amazonaws.com/0/326996/bb8147cf-e805-965a-8e6d-cd69e1f0698d.png"> |

### 事前準備

各画面を作成してコーディングの準備をします。

1. 入力画面

    1. リストセクション

        |    項目名    |  オブジェクト |      説明      |
        |--------------|---------------|----------------|
        | segSection   | Template      | テンプレート   |
        | flxSection   | FlexContainer | グループ       |
        | lblName      | Label         | タイトル       |
        | txtCounter   | TextBox       | カウンター     |
        | btnCountDown | Button        | カウントダウン |
        | btnCountUp   | Button        | カウントアップ |

        <img width="500" alt="スクリーンショット 2018-12-23 23.09.43.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/b23baafb-15e1-7e2a-9be2-a807ae2ac540.png">

    1. 入力画面

        |  項目名   |  オブジェクト |    説明    |
        |-----------|---------------|------------|
        | frmInput  | Form          | 入力画面   |
        | flxInput  | FlexContainer | グループ   |
        | lblTitle  | Label         | タイトル   |
        | segList   | Segment       | リスト     |
        | btnOutput | Button        | 出力ボタン |

        <img width="500" alt="スクリーンショット 2018-12-23 23.09.10.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/84dcd0da-d7f4-9a9f-6619-aedf7cd8a152.png">

1. 出力画面

    |  項目名   |  オブジェクト |    説明    |
    |-----------|---------------|------------|
    | frmOutput | Form          | 出力画面   |
    | flxOutput | FlexContainer | グループ   |
    | lblTitle  | Label         | タイトル   |
    | lblResult | Label         | 結果       |
    | btnBack   | Button        | 戻るボタン |

    <img width="500" alt="スクリーンショット 2018-12-23 23.17.48.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/0f12d2d4-cb66-3b68-7440-5a7929190ffa.png">

### リストデータ初期化

1. Projectタブ -> Mobile -> Controllers -> `frmInputController`をクリックして`define`の中にデータを初期化するコードを書きます。

    ```frmInputController.initData.js
    initData: function() {
        this.view.segList.setData([
            {
                lblName: {
                    text: "Data1"
                },
                txtCounter: {
                    text: 0
                },
                btnCountDown: {
                    onClick: this.countDown.bind(this)
                },
                btnCountUp: {
                    onClick: this.countUp.bind(this)
                }
            },
            {
                lblName: {
                    text: "Data2"
                },
                txtCounter: {
                    text: 0
                },
                btnCountDown: {
                    onClick: this.countDown.bind(this)
                },
                btnCountUp: {
                    onClick: this.countUp.bind(this)
                }
            },
            {
                lblName: {
                    text: "Data3"
                },
                txtCounter: {
                    text: 0
                },
                btnCountDown: {
                    onClick: this.countDown.bind(this)
                },
                btnCountUp: {
                    onClick: this.countUp.bind(this)
                }
            },
            {
                lblName: {
                    text: "Data4"
                },
                txtCounter: {
                    text: 0
                },
                btnCountDown: {
                    onClick: this.countDown.bind(this)
                },
                btnCountUp: {
                    onClick: this.countUp.bind(this)
                }
            },
            {
                lblName: {
                    text: "Data5"
                },
                txtCounter: {
                    text: 0
                },
                btnCountDown: {
                    onClick: this.countDown.bind(this)
                },
                btnCountUp: {
                    onClick: this.countUp.bind(this)
                }
            }
        ]);
    },

    countDown: function(btn, context) {
        alert("rowIndex=" + context.rowIndex);
    },

    countUp: function(btn, context) {
        alert("rowIndex=" + context.rowIndex);
    }
    ```

1. Projectタブ -> Mobile -> Forms -> `frmInput`をクリックします。次にPropertiesタブ -> Actionたタブの`init`の`Edit`をクリックします。

    <img width="300" alt="スクリーンショット 2018-12-23 23.31.26.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/2b0e724c-83a4-eab0-0726-f9c5d9c39f26.png">

1. Functionsの中から`Invoke Function`をクリックします。次にFunction Nameから`initData`を設定します。

    <img width="500" alt="スクリーンショット 2018-12-23 23.48.28.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/9e3a547a-929e-e232-7ca8-373853746fea.png">

### カウント処理

1. Projectタブ -> Mobile -> Controllers -> `frmInputController`をクリックして`define`の中にカウントダウンのコードを書きます。

    ```frmInputController.countDown.js
    countDown: function(btn, context) {
        var rowIndex = context.rowIndex;
        var sectionIndex = context.sectionIndex;
        var rowData = context.widgetInfo.data[rowIndex];

        if (Number(rowData.txtCounter.text) - 1 < 0) {
            return;
        }
        rowData.txtCounter.text = Number(rowData.txtCounter.text) - 1;

        context.widgetInfo.setDataAt(rowData, rowIndex, sectionIndex);
    }
    ```

1. 同様に`define`の中にカウントアップのコードを書きます。

    ```frmInputController.countUp.js
    countUp: function(btn, context) {
        var rowIndex = context.rowIndex;
        var sectionIndex = context.sectionIndex;
        var rowData = context.widgetInfo.data[rowIndex];

        if (Number(rowData.txtCounter.text) + 1 > 20) {
            return;
        }
        rowData.txtCounter.text = Number(rowData.txtCounter.text) + 1;

        context.widgetInfo.setDataAt(rowData, rowIndex, sectionIndex);
    }
    ```

### 出力処理

1. Projectタブ -> Mobile -> Controllers -> `frmInputController`をクリックして`define`の中に出力処理のコードを書きます。frmOutputをインスタンス化して、`navigate`で画面遷移とリストデータ(JSON)の引き渡しを実装します。

    ```frmInputController.viewOutput.js
    viewOutput: function() {
        var frmOutput = new kony.mvc.Navigation("frmOutput");
        frmOutput.navigate({data: this.view.segList.data});
    }
    ```

1. Projectタブ -> Mobile -> Controllers -> `frmOutputController`をクリックして`define`の中に出力処理のコードを書きます。`onNavigate`で画面遷移のイベントを受け取り、引き渡されたデータ(JSON)を加工して出力します。

    ```frmOutputController.onNavigate.js
    onNavigate: function(params) {
        this.view.lblResult.text = "none";
        for (var data of params.data) {
            if (Number(data.txtCounter.text) === 0) {
                continue;
            }
            if (this.view.lblResult.text == "none") {
                this.view.lblResult.text = data.lblName.text;
            } else {
                this.view.lblResult.text += "\n" + data.lblName.text;
            }
            this.view.lblResult.text += "\t : " + data.txtCounter.text;
        }
    }
    ```

1. Projectタブ -> Mobile -> Forms -> frmInput -> `btnOutput`をクリックします。次にPropertiesタブ -> Actionたタブの`onClick`の`Edit`をクリックします。

    <img width="300" alt="スクリーンショット 2018-12-23 21.40.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/35acd232-98be-7e47-365a-cf5db506e425.png">

1. Functionsの中から`Invoke Function`をクリックします。次にFunction Nameから`viewOutput`を設定します。

    <img width="500" alt="スクリーンショット 2018-12-24 21.16.59.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/f8b28a62-35ff-d0db-d46d-69540f09136e.png">

1. Projectタブ -> Mobile -> Forms -> frmOutput -> `btnBack`をクリックします。次にPropertiesタブ -> Actionたタブの`onClick`の`Edit`をクリックします。

    <img width="300" alt="スクリーンショット 2018-12-23 21.40.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/35acd232-98be-7e47-365a-cf5db506e425.png">

1. Navigationの中から`Navigate to Form`をクリックします。次に`frmInput`を設定します。

    <img width="500" alt="スクリーンショット 2018-12-24 21.35.07.png" src="https://qiita-image-store.s3.amazonaws.com/0/326996/ef76e816-a946-1635-06ca-0d0e9158c1fb.png">
