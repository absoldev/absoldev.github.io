---
layout: post
title: PythonでJSONを扱う
subtitle: Python JSON module
gh-repo: junenu
gh-badge: [follow]
tags: [python,json]
comments: true
---

# Pythonで扱うJSON

JSON (JavaScript Object Notation) は、データ交換形式として幅広く使用されています。Pythonには、このJSONを効率的に扱うための標準ライブラリ、`json`モジュールが備わっています。

## `json`モジュールの概要

`json`モジュールを利用することで、次の操作が可能になります：

- PythonオブジェクトをJSON文字列に**エンコード**
- JSON文字列をPythonオブジェクトに**デコード**

この変換は、APIのレスポンスや設定ファイルとしてよく使われます。

## エンコード: PythonからJSONへ

### `json.dumps()`

Pythonのデータ型（例: ディクショナリやリスト）をJSON文字列に変換します。

```python
import json

person = {
    'name': 'Alice',
    'age': 30,
    'hobbies': ['reading', 'cycling']
}
# person is a dictionary
print(type(person))

json_string = json.dumps(person)
print(json_string)

# json_string is a string
print(type(json_string))
```
実行結果
```bash
python3 dumps.py
<class 'dict'>
{"name": "Alice", "age": 30, "hobbies": ["reading", "cycling"]}
<class 'str'>
```

#### オプションの指定

`dumps()`はいくつかのオプションを持っています。例えば、`indent`オプションを使って、見やすく整形されたJSONを出力することができます。

```python
json_string = json.dumps(person, indent=4)
print(json_string)
```

## デコード: JSONからPythonへ

### `json.loads()`

JSON形式の文字列をPythonのデータ型に変換します。

```python
import json

json_data = '{"city": "Tokyo", "population": 13650000}'
print(type(json_data))  # <class 'str'>

data = json.loads(json_data)
print(data["city"])  # Tokyo
print(type(data))  # <class 'dict'>
```

実行結果
```bash
<class 'dict'>
{"name": "Alice", "age": 30, "hobbies": ["reading", "cycling"]}
<class 'str'>
```

## ファイルとのやり取り

### `json.dump()`

Pythonのデータ型をJSONフォーマットのファイルとして保存します。

```python
import json

person = {'name': 'Charlie', 'age': 40, 'job': 'Engineer'}
print(type(person)) # <class 'dict'>

with open('person.json', 'w') as f:
    json.dump(person, f, indent=4)
```
生成されたjsonファイル`person.json`
```json
{
    "name": "Charlie",
    "age": 40,
    "job": "Engineer"
}
```

### `json.load()`

JSONフォーマットのファイルを読み込み、Pythonのデータ型として利用します。<br>
読み込ませるjsonファイル`person2.json`
```json
{
    "name": "John",
    "age": 32,
    "job": "Developer"
}
```

```python
import json

with open('person2.json', 'r') as f:
    loaded_person = json.load(f)

print(loaded_person['name']) #John
```

## 注意点

- `json`モジュールは、Pythonのデータ型とJSONの型の間での変換をサポートしていますが、すべてのPythonデータ型がサポートされているわけではありません。
- 例えば、`datetime`オブジェクトはデフォルトではサポートされていません。カスタムエンコード・デコードが必要になる場合もあります。

