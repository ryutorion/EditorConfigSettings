# EditorConfigSettings

[EditorConfig公式](https://editorconfig.org/)

## Visual StudioのEditorConfigに関するドキュメント

- [EditorConfig で一貫性のあるコーディング スタイルを定義する()](https://learn.microsoft.com/ja-jp/visualstudio/ide/create-portable-custom-editor-options?view=vs-2022)
- [C++ EditorConfig の書式規則](https://learn.microsoft.com/ja-jp/visualstudio/ide/cpp-editorconfig-properties?view=vs-2022)
- [.editorconfigを使用してインクルードクリーンアップを構成する](https://learn.microsoft.com/ja-jp/cpp/ide/include-cleanup-config?view=msvc-170#configure-include-cleanup-with-editorconfig)

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

## cpp_indent_braces

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

## cpp_space_before_function_open_parenthesis

関数名と仮引数の丸括弧の前の空白の扱いを指定

設定できる値

- insert
  - スペースを1つ入れる
- remove
  - スペースを削除する
- ignore
  - 無視する

```cpp
// cpp_space_before_function_open_parenthesis = insert
void f ()

// cpp_space_before_function_open_parenthesis = remove
void f()

// cpp_space_before_function_open_parenthesis = ignore
// 好きな数の空白を入れられる
void f    ()
```

### 個人的な好み

```
cpp_space_before_function_open_parenthesis = remove
```

関数名から即丸括弧が並んでいる方が一体感があって，関数の一部と認識しやすい

## cpp_space_within_parameter_list_parentheses

引数リストの前後にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_parameter_list_parentheses = true
void f( int a, int b ){}
void g()
{
    f( 2, 3 );
}

// cpp_space_within_parameter_list_parentheses = false
void f(int a, int b){}
void g()
{
    f(2, 3);
}
```

### 個人的な好み

```
cpp_space_within_parameter_list_parentheses = false
```

調整中．もしかしたらtrueの方が良いかも? その場合はこの後のcpp_space_within_control_flow_statement_parenthesesもtrueにすること

## cpp_space_between_empty_parameter_list_parentheses

引数リストが空の場合にスペースを入れる

```cpp
// cpp_space_between_empty_parameter_list_parentheses = true
void f( );
// cpp_space_between_empty_parameter_list_parentheses = false
void f();
```

### 個人的な好み

```
cpp_space_between_empty_parameter_list_parentheses = false
```

下手にスペースを入れると何かの書きかけかなと思ってしまうので

## cpp_space_after_keywords_in_control_flow_statements

if/while/forなどのフロー制御の後ろにスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_after_keywords_in_control_flow_statements = true
if (true) {}

// cpp_space_after_keywords_in_control_flow_statements = false
if(false) {}
```

### 個人的な好み

```
cpp_space_after_keywords_in_control_flow_statements = false
```

関数と同じくキーワードから丸括弧までをまとめて認識したい

## cpp_space_within_control_flow_statement_parentheses

if/while/forなどのフロー制御の丸括弧内の先頭と終端に空白を入れるならture，入れないならfalse

```cpp
// cpp_space_within_control_flow_statement_parentheses = true
for( int i = 0; i < 10; ++i ){}

// cpp_space_within_control_flow_statement_parentheses = false
for(int i = 0; i < 10; ++i){}
```

### 個人的な好み

```
cpp_space_within_control_flow_statement_parentheses = false
```

若干悩ましいので，こちらをtrueにする場合はcpp_space_within_parameter_list_parenthesesもtrueにすること

## cpp_space_before_lambda_open_parenthesis

lambdaの[]と()の間に空白を入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_lambda_open_parenthesis = true
[] (){}

// cpp_space_before_lambda_open_parenthesis = false
[](){}
```

### 個人的な好み

```
cpp_space_before_lambda_open_parenthesis = false
```

まとめて認識したいので

## cpp_space_within_cast_parentheses

C形式のキャストの開き括弧の直後と閉じ括弧の直前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_cast_parentheses = true
( double )2

// cpp_space_within_cast_parentheses = false
(double)2
```

### 個人的な好み

```
cpp_space_within_cast_parentheses = false
```

そもそもC形式のキャストを使うなというところだが，C形式のキャストにする場合にスペースがあると認識しづらい

## cpp_space_after_cast_close_parenthesis

C形式のキャストの直後にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_after_cast_close_parenthesis = true
(double) 2

// cpp_space_after_cast_close_parenthesis = false
(double)2
```

### 個人的な好み

```
cpp_space_after_cast_close_parenthesis = false
```

下手に空白を入れられるとキャスト対象とまとまっていると認識しづらいので

## cpp_space_within_expression_parentheses

丸括弧で囲まれた式の前後にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_expression_parentheses = true
( ( 1 + 2 ) * 3 )

// cpp_space_within_expression_parentheses = false
((1 + 2) * 3)
```

### 個人的な好み

```
cpp_space_within_expression_parentheses = true
```

若干悩ましく，これをtrueにするなら引数リストもtrueにすべきな気がするが，引数リストは丸括弧がネストすることは無いのにたいして，
式はネストすることがあって空白を入れた方が見やすかったのでtrueにする

## cpp_space_before_block_open_brace

ブロックの開き括弧の前に空白を入れるならtrue，入れないならfalse

```
// cpp_space_before_block_open_brace = true
if() {}

// cpp_space_before_block_open_brace = false
if(){}
```

### 個人的な好み

```
cpp_space_before_block_open_brace = false
```

そもそもcpp_new_line_before_open_brace_blockで次の行になるようにしているので空白は不要

## cpp_space_between_empty_braces

空の中括弧の間にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_between_empty_braces = true
if(){ }
// cpp_space_between_empty_braces = false
if(){}
```

### 個人的な好み

```
cpp_space_between_empty_braces = false
```

下手に空白があると書きかけに見えるので

## cpp_space_before_initializer_list_open_brace

初期化子リストの開き括弧の前に空白を入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_initializer_list_open_brace = true
std::vector v { 1, 2, 3, 4, 5 };

// cpp_space_before_initializer_list_open_brace = false
std::vector v { 1, 2, 3, 4, 5 };
```

### 個人的な好み

```
cpp_space_before_initializer_list_open_brace = true
```

識別子と初期化子リストは分けて認識したいので

## cpp_space_within_initializer_list_braces

初期化子リストの開き括弧の直後と閉じ括弧の前に空白を入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_initializer_list_braces = true
std::vector v { 1, 2, 3, 4, 5 };

// cpp_space_within_initializer_list_braces = false
std::vector v {1, 2, 3, 4, 5};
```

### 個人的な好み

```
cpp_space_within_initializer_list_braces = true
```

若干こちらの方が読みやすかったので．  
ただ，初期化リストも改行するルールにするとfalseになる?

## cpp_space_preserve_in_initializer_list

初期化子リスト内の空白を維持するならtrue，維持しないならfalse

```cpp
// cpp_space_preserve_in_initializer_list = true
vector v {  1,  2,  3,
           -1, -2, -3
};

// cpp_space_preserve_in_initializer_list = false
vector v { 1, 2, 3,
    -1, -2, -3
};
```

### 個人的な好み

```
cpp_space_preserve_in_initializer_list = true
```

負数と正数で数値の開始を揃えるなど，若干整形したいこともあるのでtrue

## cpp_space_before_open_square_bracket

配列アクセスなどの四角括弧の開きの前に空白を入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_open_square_bracket = true
vector v;
v [0];

// cpp_space_before_open_square_bracket = false
vector v;
v[0];
```

### 個人的な好み

```
cpp_space_before_open_square_bracket = false
```

識別子とのかたまりとして認識したいので

## cpp_space_within_square_brackets

四角括弧の開きの後と閉じの前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_square_brackets = true
vector v;
v[ 0 ];

// cpp_space_within_square_brackets = false
vector v;
v[0];
```

### 個人的な好み

```
cpp_space_within_square_brackets = false
```

四角括弧の中にそこまで複雑な式を書くことは無いので，ひとまとめとして認識したい

## cpp_space_before_empty_square_brackets

空の四角括弧の前に空白を入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_empty_square_brackets = true
int a [];

// cpp_space_before_empty_square_brackets = false
int a[];
```

### 個人的な好み

```
cpp_space_before_empty_square_brackets = false
```

識別子とまとめて認識したいので

## cpp_space_between_empty_square_brackets

空の四角括弧の中にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_between_empty_square_brackets = true
int a[ ];

// cpp_space_between_empty_square_brackets = false
int a[];
```

### 個人的な好み

```
cpp_space_between_empty_square_brackets = false
```

無駄なスペースがあると書きかけに見えるので

## cpp_space_group_square_brackets

多次元配列の四角括弧をグループ化し，スペースを削除するならtrue，しないならfalse  
他と違ってtrueがスペースの削除なので注意

```cpp
// cpp_space_group_square_brackets = true
int a[1][2];

// cpp_space_group_square_brackets = false
int a[1] [2];
```

### 個人的な好み

```
cpp_space_group_square_brackets = true
```

グループになっている方がまとまって認識しやすいので

## cpp_space_within_lambda_brackets

lambda式のキャプチャの開きの後と閉じの前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_within_lambda_brackets = true
int x;
[ x ](){}

// cpp_space_within_lambda_brackets = false
int x;
[x](){}
```

### 個人的な好み

```
cpp_space_within_lambda_brackets = true
```

複数のキャプチャがある場合には空白があった方が読みやすそうだったので

## cpp_space_between_empty_lambda_brackets

lambda式の空のキャプチャにスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_between_empty_lambda_brackets = true
[ ](){};

// cpp_space_between_empty_lambda_brackets = false
[](){};
```

### 個人的な好み

```
cpp_space_between_empty_lambda_brackets = false
```

スペースがあると書き忘れに見えるので

## cpp_space_before_comma

カンマの前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_comma = true
void f(int a , int b);

// cpp_space_before_comma = false
void f(int a, int b);
```

### 個人的な好み

```
cpp_space_before_comma = false
```

カンマの前に空白があると別途認識する必要があって読みにくく感じたので

## cpp_space_after_comma

カンマの後にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_after_comma = true
void f(int a, int b);

// cpp_space_after_comma = false
void f(int a,int b);
```

### 個人的な好み

```
cpp_space_after_comma = true
```

カンマの後にはスペースを入れて，次の要素の始まりを認識しやすくしたい

## cpp_space_remove_around_member_operators

メンバー演算子(./->)の前後のスペースを除去するならtrue，しないならfalse

除去するだけで，falseの場合にスペースを追加するわけではないことに注意

```cpp
// cpp_space_remove_around_member_operators = true
std::vector<int> v;
v.size();
(&v)->size();

// cpp_space_remove_around_member_operators = false
std::vector<int> v;
v . size();
(&v) -> size();
```

### 個人的な好み

```
cpp_space_remove_around_member_operators = true
```

オブジェクトからのメンバアクセスは一連のものとして見たいので

## cpp_space_before_inheritance_colon

継承のコロンの前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_inheritance_colon = true
class X : public Y {};

// cpp_space_before_inheritance_colon = false
class X: public Y {};
```

### 個人的な好み

```
cpp_space_before_inheritance_colon = true
```

クラス名と継承開始のコロンは別のものとして認識したいので

## cpp_space_before_constructor_colon

コンストラクタのメンバ初期化リストのコロンの前にスペースを入れるならtrue，入れないならfalse

```cpp
// cpp_space_before_constructor_colon = true
X() : mA() {}

// cpp_space_before_constructor_colon = false
X(): mA() {}
```

### 個人的な好み

```
cpp_space_before_constructor_colon = true
```

メンバ変数初期化リストはそもそも改行必須にしたいところだが，名前の一部にしたくないのでスペースは必須に

## cpp_space_remove_before_semicolon

セミコロンの前のスペースを除去するならtrue，除去しないならfalse

あくまでも除去するだけでfalseだとスペースを追加するわけではないことに注意

```cpp
// cpp_space_remove_before_semicolon = true
int x;

// cpp_space_remove_before_semicolon = false
int x ;
```

### 個人的な好み

```
cpp_space_remove_before_semicolon = true
```

セミコロンの前に無駄なスペースは入れて欲しくない

## cpp_space_after_semicolon

セミコロンの後にスペースを入れるならtrue，入れないならfalse  
主にforを考慮したものと思われる．

```cpp
// cpp_space_after_semicolon = true
for(int i = 0; i < 10; ++i) {}

// cpp_space_after_semicolon = false
for(int i = 0;i < 10;++i) {}
```

### 個人的な好み

```
cpp_space_after_semicolon = true
```

セミコロンの後に他のものを続けると，かたまりとして認識してしまうため．

## cpp_space_remove_around_unary_operator

単項演算子と対象の間の空白を除去するならtrue，除去しないならfalse  
他と同じく除去系は取り除くだけでスペースを追加しないので注意

アラウンドなのは，インクリメントのように前のケースと後ろのケースがあるため

```cpp
// cpp_space_remove_around_unary_operator = true
int i;
i++;
++i;

// cpp_space_remove_around_unary_operator = false
int i;
i ++;
++ i;
```

### 個人的な好み

```
cpp_space_remove_around_unary_operator = true
```

## cpp_space_around_binary_operator

二項演算子の前後のスペースの扱いを指定する

設定できる値

- insert
  - スペースを挿入する
- remove
  - スペースを除去する
- ignore
  - 何もしない

```cpp
// cpp_space_around_binary_operator = insert
1 + 2;

// cpp_space_around_binary_operator = remove
1+2;

// cpp_space_around_binary_operator = ignore
1+ 2;
```

### 個人的な好み

```
cpp_space_around_binary_operator = insert
```

演算子の前後には空白を入れて区別したい

## cpp_space_around_assignment_operator

代入演算子の前後のスペースの扱いを指定する

設定できる値

- insert
  - スペースを挿入する
- remove
  - スペースを除去する
- ignore
  - 何もしない

```cpp
// cpp_space_around_assignment_operator = insert
int i = 0;
// cpp_space_around_assignment_operator = remove
int i=0;
// cpp_space_around_assignment_operator = ignore
int i= 0;
```

### 個人的な好み

```
cpp_space_around_assignment_operator = insert
```

演算子の前後には空白を入れておきたい

## cpp_space_pointer_reference_alignment

設定できる値

- left
  - 左揃え，つまり型名側に付ける
- center
  - 中央揃え，つまり両側にスペースを入れる
- right
  - 右揃え，つまり変数名側に付ける
- ignore
  - 何もしない

```cpp
// cpp_space_pointer_reference_alignment = left
int* p, & r;

// cpp_space_pointer_reference_alignment = center
int * p, & r;

// cpp_space_pointer_reference_alignment = right
int *p, &r;

// cpp_space_pointer_reference_alignment = ignore
int *p, & r;
```

### 個人的な好み

```
cpp_space_pointer_reference_alignment = right
```

そもそもすべきではないけれど，同じ行で複数の変数を宣言する場合，int *と書いていても，2つ目の変数はポインタにならないというような分かりにくい構文なので，変数名に*や&を付けて，その変数がポインタ，参照と分かりやすくした方が良い

## cpp_space_around_ternary_operator

条件演算子の前後の空白の扱いを指定する

設定できる値

- insert
  - スペースを挿入する
- remove
  - スペースを除去する
- ignore
  - 何もしない

```cpp
// cpp_space_around_ternary_operator = insert
c ? 0 : 1;

// cpp_space_around_ternary_operator = remove
c?0:1;

// cpp_space_around_ternary_operator = ignore
c  ? 0 : 1;
```

### 個人的な好み

```
cpp_space_around_ternary_operator = insert
```

条件演算子のスペースを除去すると区切りが分かりにくいのでスペースを入れた方が良い


## cpp_wrap_preserve_blocks

ブロックの改行を指定する

設定できる値

- one_liners
  - 制御文の場合は制御キーワードも含めて1行にあれば折り返さない
- all_one_line_scopes
  - 開きと閉じの括弧が同じ行にある場合はブロックに改行を入れない
- never
  - 常にブロックの開きの後と閉じの前に改行する

```cpp
// cpp_wrap_preserve_blocks = one_liners
// この2つのように1行にあれば改行はされない
{ return 0; }
if(...) { return 0; }

// それ以外のパターンは次のようになる
if(...)
{
	return 0;
}

// cpp_wrap_preserve_blocks = all_one_line_scopes
// この2つのように1行にあれば改行はされない
{ return 0; }
if(...) { return 0; }
// ifなどのブロックだけ同じ行にある場合もOK
if(...)
{ return 0; }

// それ以外のパターンは次のようになる
if(...)
{
	return 0;
}

// cpp_wrap_preserve_blocks = never
// 常に改行される
{
    return 0;
}

if(...)
{
    return 0;
}
```

### 個人的な好み

```
cpp_wrap_preserve_blocks = never
```

ブロックは常に改行有りにすることで，後から位置が変わって差分が分かりにくくなるといったことが避けられる
