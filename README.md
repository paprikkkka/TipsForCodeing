# TipsForCodeing

How to edit Readme\
https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

## Git

### Git ローカル環境のファイル変更無視
git コマンド

ファイル監視状況
git ls-files -v

特定のファイルを無視、監視
git update-index --assume-unchanged 「ファイルパス」

git update-index --no-assume-unchanged


### Git hooks

#### 特定のブランチにコミットできないように

リポジトリの「.git/hooks」内「pre-commit」ファイルを作成。

ファイル内下記内容記入する
```
#!/bin/bash

echo -e " Running Git Hooks: pre-commit"

# 現在のブランチ名を定義
BRANCH_NAME=$(git rev-parse --abbrev-ref HEAD)
readonly BRANCH_NAME
# 終了コードを定義。0: OK, 1: NG
code=0

# コミットするブランチ名のチェック
echo -en " - コミットするブランチ名のチェック: "
## コミットを拒否するブランチ名を定義
## コミットしたくないブランチ名をINCORRECT_BRANCHESに追加する
readonly INCORRECT_BRANCHES=("master" "main" "JFX600Common_dev")
if echo "${INCORRECT_BRANCHES[@]}" | grep -q "$BRANCH_NAME"; then
  echo -e "NG"
  echo -e "================================================================"
  echo -e "${BRANCH_NAME}ブランチへのコミットは禁止されています。"
  echo -e ""
  echo -e "禁止されているブランチ"
  echo -e "  ${INCORRECT_BRANCHES[*]}"
  echo -e "================================================================"
  code=1
else
  echo -e "OK"
fi

# 終了宣言
if [ ${code} -eq 0 ]; then
  echo ""
  echo -e "\033[32;1m✨ALL PASS!!\033[0m\n\n"
else
  echo ""
  echo -e "\033[31;1mGit Hooks: pre-commit: NG\033[0m\n\n"
fi

exit ${code}

```

ファイル実行権限を与える。
$ chmod a+x .git/hooks/pre-commit



## C#

### DeepCopy

**Useing BinaRyFormatter**

https://qiita.com/gazf/items/5d5393ac37c2304ecf0a

```
public static T BinaryFomartterClone<T>(T src)
{
    using (var ms = new MemoryStream())
    {
        var binaryFormatter = new System.Runtime.Serialization
            .Formatters.Binary.BinaryFormatter();
        binaryFormatter.Serialize(ms, src);
        ms.Seek(0, SeekOrigin.Begin);
        return (T)binaryFormatter.Deserialize(ms);
    }
}
```
