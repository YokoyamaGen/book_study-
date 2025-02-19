
 ### 第1章 メタプログラミングRuby
  - メタプラグラミングとはコードを記述するためのコードを記述することである。例えば、Active Recordでメタプログラミングが使用されている。ActiveRecord::Baseクラスを継承するだけで、継承したクラスをインスタンス化して、テーブルのカラムを呼び出したり、カラムのデータを更新できる。ActiveRecord::Baseクラスを継承することで、継承元のコードでアクセッサメソッドを利用できるようになるため、コードを記述するためのコードという。

  - コードの重複を排除できるメリットがある。

 ### 第2章 オブジェクトモデル
  - オープンクラス
    - 例えば、文字列の中で#のような特殊文字列を除外するためのメソッドを定義して、文字列を渡すよりもStringクラスの中で特殊文字列を除外するためのメソッドを定義する方がよりオブジェクト指向の実装になる。しかし、デメリットとして、すでにクラスに定義されているメソッド名でメソッドを定義してまうと既存のメソッドの中身が書き変わってしまい、想定外のエラーが発生してしまう。

    - オブジェクトの中にインスタンス変数が存在している。そのため、オブジェクトによって、インスタンス変数の中身が異なる。メソッドはクラスに存在している。着目しているのがオブジェクトの場合、メソッドと呼び、クラスに着目している場合、インスタンスメソッドと呼び分ける。

    - メソッドを探索する流れはレシーバからクラスを見に行き、該当のメソッドが存在しなければ親クラスを参照していく。ちなみにモジュールをクラスがincludeするとクラスの親にincludeしたモジュールが設定される。クラスでモジュールをprependするとクラスのサブクラスにprependしたモジュールが設定される。

    - メソッドの実行時にはレシーバであるオブジェクトのメモリアドレス情報をselfに保存しておく。selfに保存しておくことでオブジェクトの情報を参照することができる。メソッド呼び出しにselfを指定しなくても暗黙的に指定される。privateメソッドは明示的にselfをつけると外部オブジェクトからの呼び出されているため、privateメソッドを呼び出せない。そのため、privateメソッドを呼び出すためにはselfはつけない。

    - オープンラクスを使用するとグローバルにメソッドの定義を変更してしまうデメリットがある。しかし、モジュールにRefinementsで定義して、それをusingで使用すると指定した範囲でのみメソッドを上書きすることができる。

 ### 第3章 メソッド

 - send（メソッドを動的に呼び出す）
    - メソッドを動的に渡すことで、重複コードを排除できる。メソッドを呼び出す際に「インスタンス名.メソッド名」のように書く。sendメソッドを使用すれば、「インスタンス名.send(メソッド名)」。同じような構造のコードの中で、複数のメソッド名を配列の中に格納しておき、ループで渡すことでメソッドを呼び出すことができる。

 - define_method（メソッドを動的に定義する）
    - defメソッド名と定義するのではなく、define_methodを使用して、メソッドを定義することにより、実行時にメソッド名を決められる。メソッド名が一部異なる複数のメソッドを記載する際に重複を省くことができる。例えば、get_cpu_info、get_mouse_infoなどのget_xxxx_infoにおけるxxxxを動的に渡すことで、定義できる。

  - method_missing(ゴーストメソッド)
    - レシーバに存在しないメソッドを呼び出すと親クラスのBasicObjectクラスから継承したmethod_missingメソッドが呼び出されて、エラーメッセージが表示される。その特性を活かして、method_missingメソッドをオーバライドすることによって、冗長なコードを簡潔なコードにする。ただし、method_missingメソッドをオーバライドしたメソッドの中で変数のスコープに関する文法的なエラーが発生した場合や親クラスですでに定義済みのメソッドをレシーバで呼び出そうとした際のエラーが発生した際の対応が困難になる。その対応策として、method_missingメソッドの中でsuperを使用することで、method_missingメソッド本来の動きをすることによって、文法エラーが発生しても検知できるようにする。また、親クラスにすでに存在しているメソッドを呼び出された場合、想定通りの動作をしない場合、親クラスをObjectクラスではなく、BasicObjectクラスを継承するようにする。Objectクラスには多くのメソッドが定義されているため、その親クラスであるBasicObjectクラスを直接継承することで不要なメソッドをレシーバから呼び出さないようにする。

  - 冗長なコードを簡潔に記述するために動的メソッドとゴーストメソッドの方法があるが、可能であれば動的メソッドを使用する。動的メソッドでできなければゴーストメソッドを使用する。なぜならゴーストメソッドでエラーが発生すると解決する難易度が高いため、動的メソッドの方を利用した方が良い。
