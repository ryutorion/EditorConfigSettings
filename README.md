# EditorConfigSettings

[EditorConfig公式](https://editorconfig.org/)

## Visual StudioのEditorConfigに関するドキュメント

- [EditorConfig で一貫性のあるコーディング スタイルを定義する()](https://learn.microsoft.com/ja-jp/visualstudio/ide/create-portable-custom-editor-options?view=vs-2022)
- [C++ EditorConfig の書式規則](https://learn.microsoft.com/ja-jp/visualstudio/ide/cpp-editorconfig-properties?view=vs-2022)

## root
EditorConfigはルート(Windowsならドライブ直下，それ以外は/)に辿り付くまでか，root=trueと書かれた.editorconfigを見つけるまで，途中のフォルダ/ディレクトリで見つかった.editorconfigを読み込んでいく．ただし，設定はフォーマット対象のファイルに近い階層にある.editorconfigが優先される．

## indent_style

インデントにタブ文字を使うか空白文字を使うかどうかを指定できる．

- 設定可能な値
  - tab
  - space

```cpp
// printfの前の空白を選択すると文字数がtabは1つだと分かる

// indent_style = space
int main()
{
    printf("");
}

// indent_stye = tab
int main()
{
	printf("");
}
```

### 個人的な好み

#### C++

```
indent_style = tab
```

タブにしたい．  
タブ文字の表示幅はエディタによって変更可能なので，見やすい長さに設定しておけばよい．  
タブにしたい理由としては，ソースコードの量が多くなってくるとインデントの量も増えてくるので，少しでも文字数が少ない方がよい気がするからである．  
コンパイラの処理的にも4文字とかの空白を処理するより，1文字のタブを処理する方が若干速いだろう．

#### C#

```
indent_style = space
```

[C#のコーディング規則にあるスタイルのガイドライン](https://learn.microsoft.com/ja-jp/dotnet/csharp/fundamentals/coding-style/coding-conventions#style-guidelines)でタブを使用せずに4つのスペースを使う，と強く書いてあるのでそれに従う．
 
## indent_size

インデントの幅を数値で設定する．indent_styleがtabの場合は無視される．

### 個人的な好み

#### C++

indent_styleがtabなので無関係．

#### C#

```
indent_size = 4
```

[C#のコーディング規則にあるスタイルのガイドライン](https://learn.microsoft.com/ja-jp/dotnet/csharp/fundamentals/coding-style/coding-conventions#style-guidelines)により4になる．

## tab_width

### 個人的な好み

#### C++

```
tab_width = 4
```

8は深すぎるし，2だと浅すぎるので4がちょうど良い感じに見えます．

#### C#

indent_styleがspaceなので無関係

## end_of_line

改行コードを設定する．プラットフォームによるので設定しない方が良いのか?

設定できる値

- lf
- cr
- crlf

## charset

文字コードを設定する

設定できる値

- latin1
- utf-8
- utf-8-bom
- utf-16be
- utf-16le

### 個人的な好み

#### C++

```
charset = utf-8-bom
```

Visual Studioは日本語環境ではBOM有りのUTF-8にしないと日本語コメントなどを正しく認識しないことがあるので選択肢が無い．
/source-charsetか/utf-8オプションを指定する場合はその限りではないので，utf-8にする．  
プロジェクトの途中から/utf-8に変えると文字コード関連の問題を引く可能性があるので，設定するなら最初から設定する．

#### C#

```
charset = utf-8
```

## trim_trailing_whitespace

改行の前の空白を削除するならtrue，しないならfalse

### 個人的な好み

```
trim_trailing_whitespace = true
```

改行前の無駄な空白は不要なのでtrue

## insert_final_newline

ファイルの終端を改行文字にするならtrue，そうでなければfalse

### 個人的な好み

```
insert_final_newline = true
```

Vimがそうなので

## cpp_generate_documentation_comments

ドキュメンテーションコメントの形式を指定する．
自動生成には出てくるが，ドキュメントについてはvs_の形式のものしかないし，ブログでしか言及されていない．

[Doxygen and XML Doc Comment support](https://devblogs.microsoft.com/cppblog/doxygen-and-xml-doc-comment-support/)

Visual Studio 2022 17.12では扱える値も増えている模様．

設定できる値
- none
- xml
- doxygen_triple_slash
  - /// で始まる
- doxygen_slash_star
  - /\*\* で始まる
- doxygen_slash_star_exclamation
  - /\*!で始まる
- doxygen_double_slash_exclamation
  - //!で始まる

### 個人的な好み

個人的にはXML形式は閉じタグが必要で面倒なので，Doxygen形式の方が好きです．

## cpp_indent_bracesg

中括弧({)を字下げする場合はtrue，しない場合はfalse

```cpp
// cpp_indent_braces = true
int main()
	{
	}

// cpp_indent_braces = false
int main()
{
}
```

### 個人的な好み

```
cpp_indent_braces = false
```

そもそもtrueなケースをあまり見ないと思います

## cpp_indent_multi_line_relative_to

丸括弧の中を複数行にした場合に，どこを基準にインデントをするかの設定

- 取りうる値
  - outermost_parenthesis
    - 一番外側の丸括弧を基準にインデント
  - innermost_parenthesis
    - 一番内側の丸括弧を基準にインデント
  - statement_begin
    - 

```cpp
// cpp_indent_multi_line_relative_to = outermost_parenthesis
value1 = 1 + OuterFunction(nullptr,
    InnerFunction(true,
    false));

value2 = 2 +
    OuterFunction(nullptr,
        InnerFunction(true, false));

value3 = 3 +
    InnerFunction(true,
        false);

// cpp_indent_multi_line_relative_to = innermost_parenthesis
value1 = 1 + OuterFunction(nullptr,
    InnerFunction(true,
        false));

value2 = 2 +
    OuterFunction(nullptr,
        InnerFunction(true, false));

value3 = 3 +
    InnerFunction(true,
        false);

// cpp_indent_multi_line_relative_to = statement_begin
value1 = 1 + OuterFunction(nullptr,
    InnerFunction(true,
    false));

value2 = 2 +
    OuterFunction(nullptr,
    InnerFunction(true, false));

value3 = 3 +
    InnerFunction(true,
    false);
```

### 個人的な好み

```
cpp_indent_multi_line_relative_to = innermost_parenthesis
```

内側の丸括弧は組み合わせが分かりやすいように更に中でインデントしたい派です

## cpp_indent_within_parentheses

丸括弧内で入力した新しい行を揃える

設定できる値
- align_to_parenthesis
  - 開き丸括弧に合わせる
- indent
  - 新しい行でインデントする

```cpp
// cpp_indent_within_parentheses = align_to_parenthesis
printf("%d",
       1
);
// cpp_indent_within_parentheses = indent
printf("%d",
    1
);
```

### 個人的な好み

```
cpp_indent_within_parentheses = indent
```

indent_styleをtabにしている場合，indent一択になる．  
align_to_parenthesisを指定すると，縦揃えのためにtabの後に空白が来るような実装になっている．

## cpp_indent_preserve_within_parentheses

cpp_indent_within_parenthesesの設定を既存のコードにも適用する場合はfalse，しない場合はtrue

### 個人的な好み

意図的に調整しても上書きされてしまうのでtrue一択

## cpp_indent_case_contents

```cpp
// cpp_indent_case_contents = true
switch(v)
{
case 0:
	break;
case 1:
	break;
}

// cpp_indent_case_contents = false
switch(v)
{
case 0:
break;
case 1:
break;
}
```

### 個人的な好み

```
cpp_indent_case_contents = true
```

case文の内容は字下げして欲しいのでtrue一択

## cpp_indent_case_labels

caseそのものを字下げするならtrue，しないならfalse

```cpp
// cpp_indent_case_labels = true
switch(v)
{
	case 0:
		break;
	case 1:
		break;
}

// cpp_indent_case_labels = false
switch(v)
{
case 0:
	break;
case 1:
	break;
}

### 個人的な好み

```
cpp_indent_case_labels = false
```

無駄にインデントが深くなるのでfalse一択

## cpp_indent_case_contents_when_block

case文中のブロックをインデントするならtrue，しないならfalse

case文内で変数を宣言したい場合はブロックを作る必要があるので，気にする必要がある

```cpp
// cpp_indent_case_contents_when_block = true
switch(v)
{
case 0:
	{
	}
	break;
}

// cpp_indent_case_contents_when_block = false
switch(v)
{
case 0:
{
}
break;
}
```

なお，falseにした場合，cpp_indent_case_contents = trueであってもブロック以外も字下げされなくなる模様．

### 個人的な好み

```
cpp_indent_case_contents_when_block = true
```

若干悩ましいが，falseにした場合に，cpp_indent_case_contentsを打ち消してしまうのがいただけないのでtrue

## cpp_indent_lambda_braces_when_parameter

lambda式の中括弧をインデントするならtrue，しないならfalse

ドキュメントにはパラメータとして使う，とあるがそうでない場合でも影響を受けている気がする

```cpp
// cpp_indent_lambda_braces_when_parameter = true
[](){
	};

// cpp_indent_lambda_braces_when_parameter = false
[](){
};
```

### 個人的な好み

```
cpp_indent_lambda_braces_when_parameter = false
```

そもそもlambdaを多用しないのだけれど，若干無駄なインデントに思えるのでfalse

## cpp_indent_goto_labels

gotoのためのラベルの位置を指定する

設定できる値
- one_left
  - 1つ左にする
- leftmost_column
  - 一番左にする
- none
  - インデントを変更しない

```cpp
// cpp_indent_goto_labels = one_left
class X
{
	void f()
	{
		goto Y;
	Y:
		return;
	}
};

// cpp_indent_goto_labels = leftmost_column
class X
{
	void f()
	{
		goto Y;
Y:
		return;
	}
};

// cpp_indent_goto_labels = none
class X
{
	void f()
	{
		goto Y;
		Y:
		return;
	}
};


```

### 個人的な好み

```
cpp_indent_goto_labels = one_left
```

2，3段のネストの中で書いた場合に左端にあると気付きにくい気がするので，1つ左で十分だと思う

## cpp_indent_preprocessor

プリプロセッサディレクティブ(#で始まる)のためのラベルの位置を指定する

設定できる値
- one_left
  - 1つ左にする
- leftmost_column
  - 一番左にする
- none
  - インデントを変更しない

```cpp
// cpp_indent_preprocessor = one_left
class X
{
	void f()
	{
	#if defined(NDEBUG)
	#endif
		return;
	}
};

// cpp_indent_preprocessor = leftmost_column
class X
{
	void f()
	{
#if defined(NDEBUG)
#endif
		return;
	}
};

// cpp_indent_preprocessor = none
class X
{
	void f()
	{
		#if defined(NDEBUG)
		#endif
		return;
	}
};
```

### 個人的な好み

```
cpp_indent_preprocessor = one_left
```

条件をネストで表現したい場合にプリプロセッサディレクティブでインデントしたいケースがあるのでone_left

## cpp_indent_access_specifiers

アクセス指定子(public/protected/private)の位置をインデントするならtrue，しないならfalse

```cpp
// cpp_indent_access_specifiers = true
class X
{
	public:
};

// cpp_indent_access_specifiers = false
class X
{
public:
};
```

### 個人的な好み

```
// cpp_indent_access_specifiers = false
```

trueにしたい理由が思いつかない

## cpp_indent_namespace_contents

名前空間の内容をインデントするならtrue，しないならfalse

```cpp
// cpp_indent_namespace_contents = true
namespace X
{
	class Y {};
}

// cpp_indent_namespace_contents = false
namespace X
{
class Y {};
}
```

### 個人的な好み

```
cpp_indent_namespace_contents = true
```

名前空間の内容をインデントしたくない理由が思いつかない

## cpp_indent_preserve_comments

書式を整える際にコメントのインデントを維持するならtrue，維持しないならfalse

```cpp
// cpp_indent_preserve_comments = true
void f()
{
	// hoge
		// fuga
	// インデントのズレが維持される
}

// cpp_indent_preserve_comments = false
void f()
{
	// hoge
	// fuga
	// インデントのズレが矯正される
}
```

### 個人的な好み

```
cpp_indent_preserve_comments = false
```

そもそもコメントの先頭を意図的にずらしたいケースが思いつかない

## cpp_new_line_before_open_brace_namespace

名前空間の開き括弧の位置を指定する

設定できる値

- new_line
  - 新しい行に移動する
- same_line
  - 同じ行でスペースを入れる
- ignore
  - 再配置しない

```cpp
// cpp_new_line_before_open_brace_namespace = new_line
namespace X
{
	namespace Y
	{
	}
}

// cpp_new_line_before_open_brace_namespace = same_line
namespace X {
	namespace Y {
	}
}

// cpp_new_line_before_open_brace_namespace = ignore
// 2パターンが混在してもOK
namespace X {
	namespace Y
	{
	}
}
```

### 個人的な好み

```
cpp_new_line_before_open_brace_namespace = new_line
```

開き括弧と閉じ括弧は縦にそろって欲しい派なので

## cpp_new_line_before_open_brace_type

型宣言の開き括弧の位置を指定する

設定できる値

- new_line
  - 新しい行に移動する
- same_line
  - 同じ行でスペースを入れる
- ignore
  - 再配置しない

```cpp
// cpp_new_line_before_open_brace_type = new_line
class X
{};

class Y
{};

// cpp_new_line_before_open_brace_type = same_line
class X {
};

class Y {
};

// cpp_new_line_before_open_brace_type = ignore
// 両タイプが混在してもOK
class X
{};

class Y {
};
```

### 個人的な好み

```
cpp_new_line_before_open_brace_type = new_line
```

開き括弧と閉じ括弧は縦にそろって欲しい派なので

## cpp_new_line_before_open_brace_function

関数定義の開き括弧の位置を指定する

設定できる値

- new_line
  - 新しい行に移動する
- same_line
  - 同じ行でスペースを入れる
- ignore
  - 再配置しない


```cpp
// cpp_new_line_before_open_brace_function = new_line
void f()
{
}

void g()
{
}

// cpp_new_line_before_open_brace_function = same_line
void f() {
}

void g() {
}

// cpp_new_line_before_open_brace_function = ignore
// 両方のパターンが混ざっていてもOK
void f()
{
}

void g() {
}
```

### 個人的な好み

```
cpp_new_line_before_open_brace_function = new_line
```

開き括弧と閉じ括弧は縦にそろって欲しい派なので

## cpp_new_line_before_open_brace_block

forやwhileなどフロー制御のブロックの開き括弧の位置を指定する  
名前からは影響を受けそうだが，通常のブロックの開き括弧の位置は影響を受けない

設定できる値

- new_line
  - 新しい行に移動する
- same_line
  - 同じ行でスペースを入れる
- ignore
  - 再配置しない

```cpp
// cpp_new_line_before_open_brace_block = new_line
// 単なるブロックは影響を受けない
{
}

while(true)
{
	if(false)
	{
	}
}

// cpp_new_line_before_open_brace_block = same_line
// 単なるブロックは影響を受けない
{
}

while(true) {
	if(false) {
	}
}


// cpp_new_line_before_open_brace_block = ignore
// 単なるブロックは影響を受けない
{
}

// 両方のパターンが混ざっていてもOK
while(true)
{
	if(false) {
	}
}
```

### 個人的な好み

```
cpp_new_line_before_open_brace_block = new_line
```

開き括弧と閉じ括弧は縦にそろって欲しい派なので

## cpp_new_line_before_open_brace_lambda

lambdaの開き括弧の位置を指定する

設定できる値

- new_line
  - 新しい行に移動する
- same_line
  - 同じ行でスペースを入れる
- ignore
  - 再配置しない


```cpp
// cpp_new_line_before_open_brace_lambda = new_line
auto lambda0 = []()
{
};

auto labmda1 = []()
{
};

// cpp_new_line_before_open_brace_lambda = same_line
auto lambda0 = []() {
};

auto labmda1 = []() {
};


// cpp_new_line_before_open_brace_lambda = ignore
// 両方のパターンが混ざっていてもOK
auto lambda0 = []()
{
};

auto labmda1 = []() {
};
```

### 個人的な好み

```
cpp_new_line_before_open_brace_lambda = new_line
```

若干悩ましいが，ここまで来ると全部開き括弧は改行で揃えたい

## cpp_new_line_scope_braces_on_separate_lines

説明はスコープの中かっこを別の行に配置する，とあるが，
スコープを定義するブロックの内容を別の行から開始するか，と解釈するのが良さそう

trueなら別の行に，falseなら同じ行にする


```cpp
// cpp_new_line_scope_braces_on_separate_lines = true
{
	int i;
}

// cpp_new_line_scope_braces_on_separate_lines = false
{ int i;
}
```

### 個人的な好み

```
cpp_new_line_scope_braces_on_separate_lines = true
```

ブロックの開始に内容があると読みにくいです

## cpp_new_line_close_brace_same_line_empty_type

型宣言が空の場合に閉じ括弧を同じ行に書くならtrue，書かないならfalse

```cpp
// cpp_new_line_close_brace_same_line_empty_type = true
class X
{};

// 実質空でもアクセス指定子やコメントがあれば別行になる
class Y
{
public:
};

// cpp_new_line_close_brace_same_line_empty_type = false
class X
{
};

// 実質空でもアクセス指定子やコメントがあれば別行になる
class Y
{
public:
};
```

### 個人的な好み

```cpp
cpp_new_line_close_brace_same_line_empty_type = false
```

後から書き足すこともあるので，入力中にフォーマットが入ると邪魔になったから


## cpp_new_line_close_brace_same_line_empty_function

関数定義が空の場合に閉じ括弧を同じ行に書くならtrue，書かないならfalse

```cpp
// cpp_new_line_close_brace_same_line_empty_function = true
void f()
{}

void g
{
	// コメントなどがあれば空扱いされない
}

// cpp_new_line_close_brace_same_line_empty_function = false
void f()
{
}

void g
{
	// コメントなどがあれば空扱いされない
}
```

### 個人的な好み

```
cpp_new_line_close_brace_same_line_empty_function = false
```

後から書き足すこともあるので，入力中にフォーマットが入ると邪魔になったから

## cpp_new_line_before_catch

catchの前に改行を入れるならtrue，入れないならfalse

```cpp
// cpp_new_line_before_catch = true
try
{
}
catch(...)
{
}

// cpp_new_line_before_catch = false
try
{
} catch(...)
{
}
```

### 個人的な好み

```
cpp_new_line_before_catch = true
```

cpp_new_line_before_brace_blockとの組み合わせになるが，開き括弧と閉じ括弧のようにtry/catchも縦に並べたい

## cpp_new_line_before_else

elseの前に改行を入れるならtrue，入れないならfalse

```cpp
// cpp_new_line_before_else = true
if(true)
{
}
else
{
}

// cpp_new_line_before_else = false
if(true)
{
} else
{
}
```

### 個人的な好み

```
cpp_new_line_before_else = true
```

cpp_new_line_before_brace_blockとの組み合わせになるが，開き括弧と閉じ括弧のようにif/elseも縦に並べたい

## cpp_new_line_before_while_in_do_while

do whileのwhileの前に改行を入れるならtrue，入れないならfalse

```cpp
// cpp_new_line_before_else = true
do
{
}
while(true);

// cpp_new_line_before_else = false
do
{
} while(true);

```

### 個人的な好み

```
cpp_new_line_before_else = true
```

元々はfalseが好みだけれど，ここまで設定していて一貫性を考えると改行した方が良いのではという気がしてきたので，しばらく改行で書いてみる
