---
title: Unit Test
tags:
  - python
categories:
  - Develop
date: 2025-04-21
draft: true
---

https://dysonma.github.io/2021/01/27/Python-Unit-Testing-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6/


## code coverage in python with pytest

pip install pytest-cov pytest

pytest --cov=./ --cov-config=config_file

## CI
在 CI 過程中，先偵測變動的路徑。如果變動在 pipelines/project_A/，則只執行該目錄下的測試。

工具： pytest-changedfiles 或自定義 git diff 指令。

優點： 只有改到的 Python 邏輯會跑測試，節省 90% 以上的時間。


## pytest plugins
https://myapollo.com.tw/blog/7-useful-pytest-plugins/

## 其他套件與工具 
tox、nox

## pytest vs unit test

| 特性                            | `unittest`（標準庫）                                 | `pytest`（第三方套件）                                                                                                      |
| ------------------------------- | ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| 是否內建                        | ✅ 是（標準庫）                                      | ❌ 否（需 `pip install pytest`）                                                                                            |
| 語法簡潔性                      | 較繁瑣（需繼承 `unittest.TestCase`）                 | ✅ 非常簡潔，類似純 Python 函式                                                                                             |
| 自動發現測試                    | 部分支援（需命名為 `test*`）                         | ✅ 完整支援                                                                                                                 |
| 斷言語法                        | ❌ 使用 `self.assertEqual()` 等                      | ✅ 直接使用 `assert` 語法                                                                                                   |
| 插件與擴充性                    | 較弱                                                 | ✅ 擴充性極強（如 `pytest-mock`, `pytest-cov`）                                                                             |
| 測試報告格式                    | 基本輸出                                             | ✅ 有更豐富的輸出，可整合報告工具                                                                                           |
| 支援參數化測試                  | ❌ 須手動處理                                        | ✅ 使用 `@pytest.mark.parametrize` 非常方便                                                                                 |
| 學習曲線                        | 較陡，偏向 Java/JUnit 風格                           | ✅ 平緩，語法自然                                                                                                           |
| 測試前/後的資源設置以及後續清理 | 提供 `setUp/tearDown` 方法，但是不同測試類別不能共用 | 提供 `@pytest.fixture` 修飾子，可以自己定義 function，只要加上 `@pytest.fixture` 這個修飾子，被修飾的 function 就可以被使用 |
| 測試案例分類執行                                |      預設是要執行全部的測試案例                                                |                        可以通過`@pytest.mark` 來標記測試類別跟 function，執行測試時用 `pytest -m {你的類別}` 來執行特定的測試案例                                                                                                     |


## Fixture/Scope

- @pytest.fixture(scope="function"): 每一個測試 function 都會被創建/清理一次，每一個測試 function 的 fixture 都是獨立全新的，不互相影響。
- @pytest.fixture(scope="class"): 同一個類別內，不同的 function 會共用相同的 fixture 資源。
- @pytest.fixture(scope="module"): 是在當前 .py 檔裡面所有測試 function (包含類別)開始前執行前只執行一次，並在測試完成後清理。
- @pytest.fixture(scope="session"): 是在當前這個測試會話的前後進行設置 / 清理工作，可包含多個不同的測試 .py 檔。


## pytest.mark 將測試分類

- @pytest.mark.integration
- @pytest.mark.skip(reason="Skipping substract test")

## pytest.mark.parameterize 參數化測試

``` python
def add(a, b):
    return a + b

import pytest
from math_operations import add

@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (4, 5, 9),
    (10, -10, 0),
    (0, 0, 0)
])
def test_add(a, b, expected):
    assert add(a, b) == expected
```