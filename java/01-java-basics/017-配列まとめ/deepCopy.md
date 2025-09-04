# deepCopy

# Q,

「多次元配列を完全にコピー（ディープコピー）する方法」は、拡張forかfor文の多重ループを使用する認識でしたが、あっていますか？

はい、Hiroaki さんの認識は **正しい** です 👍

---

## ✅ ディープコピーの基本

- **`clone()` は浅いコピー（1階層分のみコピー）**
- 多次元配列を「完全コピー（ディープコピー）」したい場合は、**内側の配列もすべてコピーする必要がある**
- そのために **for文の多重ループ** か **拡張for文のネスト** を使います。

---

## 📌 例：2次元配列のディープコピー

```java
int[][] arr1 = {
    {1, 2},
    {3, 4}
};

int[][] arr2 = new int[arr1.length][];
for (int i = 0; i < arr1.length; i++) {
    arr2[i] = arr1[i].clone(); // 内側も clone
}

// arr2 を変更しても arr1 には影響なし
arr2[0][0] = 99;

System.out.println(arr1[0][0]); // 1
System.out.println(arr2[0][0]); // 99
```

### **📌 例：3次元配列のディープコピー**

```java
int[][][] arr1 = {
    { {1,2}, {3,4} },
    { {5,6}, {7,8} }
};

int[][][] arr2 = new int[arr1.length][][];
for (int i = 0; i < arr1.length; i++) {
    arr2[i] = new int[arr1[i].length][];
    for (int j = 0; j < arr1[i].length; j++) {
        arr2[i][j] = arr1[i][j].clone(); // 内側をコピー
    }
}

arr2[0][0][0] = 99;

System.out.println(arr1[0][0][0]); // 1
System.out.println(arr2[0][0][0]); // 99
```

---

## ✅ まとめ

- **多次元配列のディープコピー = for / 拡張for を使って内側の配列もコピーする**
- `clone()` や `Arrays.copyOf()` だけでは浅いコピーしかできない
- ディープコピーが必要な場合は「階層ごとにコピー」を意識する

---

👉 Hiroaki さん、これを **「浅いコピー / ディープコピーの比較表（コード付き）」** に整理すると、さらに頭に残りやすいですが、作りますか？