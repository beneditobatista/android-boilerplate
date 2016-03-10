# Android Boilerplate

Exemplo de App Android usado na [ribot](http://ribot.co.uk) como referencia para novos projetos Android. Demonstra a arquitetura, ferramentas e guidelines que usam no desenvolvimento para a plataforma Android (https://github.com/ribot/android-guidelines)

Libraries e Ferramentas incluidas:

- Support libraries
- RecyclerViews e CardViews 
- [RxJava](https://github.com/ReactiveX/RxJava) e [RxAndroid](https://github.com/ReactiveX/RxAndroid) 
- [Retrofit 2](http://square.github.io/retrofit/)
- [Dagger 2](http://google.github.io/dagger/)
- [SqlBrite](https://github.com/square/sqlbrite)
- [Butterknife](https://github.com/JakeWharton/butterknife)
- [Timber](https://github.com/JakeWharton/timber)
- [Picasso](http://square.github.io/picasso/)
- [Otto](http://square.github.io/otto/) event bus
- Testes Functionais com [Espresso](https://code.google.com/p/android-test-kit/wiki/Espresso)
- [Robolectric](http://robolectric.org/)
- [Mockito](http://mockito.org/)
- [Checkstyle](http://checkstyle.sourceforge.net/), [PMD](https://pmd.github.io/) e [Findbugs](http://findbugs.sourceforge.net/) para analise de código

## Requirementos

- [Android SDK](http://developer.android.com/sdk/index.html).
- Android [6.0 (API 23) ](http://developer.android.com/tools/revisions/platforms.html#6.0).
- Versão mais atual do Android SDK Tools e Build tools.


## Arquitetura

Este projeto segue as guidelines da Arquitetura Android da ribot, que são baseadas em [MVP (Model View Presenter)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter). Leia mais sobre isso [here](https://github.com/ribot/android-guidelines/blob/master/architecture_guidelines/android_architecture.md). 

![](https://github.com/ribot/android-guidelines/raw/master/architecture_guidelines/architecture_diagram.png)

### Como implementar uma nova tela seguindo MVP

Imagine que você precise implementar uma tela de sign-in.

1. Crie um novo package abaixo de `ui` com nome de `signin`
2. Crie uma nova Activity com o nome de `ActivitySignIn`. Você tambem pode usar um Fragment.
3. Defina a interface da view que sua Activity vai implementar. Crie uma nova interface com o nome de `SignInMvpView` que extende de `MvpView`. Adicione os metodos que achar necessarios, por ex: `showSignInSuccessful()`
4. Crie uma classe chamada `SignInPresenter` que extende de `BasePresenter<SignInMvpView>`
5. Implemente os metodos em `SignInPresenter` que serão necessarios para que sua Activity execute as ações esperadas, por ex: `signIn(String email)`. Sempre que a ação de sign terminar voce deverá chamar `getMvpView().showSignInSuccessful()`.
6. Crie um `SignInPresenterTest` e escreva testes unitarios para `signIn(email)`. Lembre-se de criar mock's para `SignInMvpView` e `DataManager`.
7. Faça sua classe `ActivitySignIn` implementar `SignInMvpView` e implemente os metodos necessarios como `showSignInSuccessful()`
8. Em sua Activity, injete a nova instancia de `SignInPresenter` e chame `presenter.attachView(this)` no `onCreate` e `presenter.detachView()` no `onDestroy()`. Configure tambem, um listener para o click do button que chama `presenter.signIn(email)`.

## Qualidade do Código

Este projeto integra a combinação entre testes unitarios, testes funcionais e ferramentas de analise de codigos.

### Testes

Para executar os testes **unitarios** em sua maquina:

``` 
./gradlew test
``` 

Para executar testes **funcionais** em devices conectados:

``` 
./gradlew connectedAndroidTest
``` 

Nota: Para usar syntax highlighting no Android Studio para testes automatizados e unitarios, você **deve** alterar a Build Variant para o modo desejado. 

### Ferramentas de Analise de Codigos 

As seguintes ferramentas de analise de codigos estão configuradas neste projeto:

* [PMD](https://pmd.github.io/): Essa ferramenta encontra falhas comuns de programação como variaveis não utilizadas, blocos catch vazios, criação desnecessaria de objetos, e assim por diante. Olhe [conjunto de regras PMD desse projeto] (config/quality/pmd/pmd-ruleset.xml).

``` 
./gradlew pmd
```

* [Findbugs](http://findbugs.sourceforge.net/): Essa ferramenta usa analise estatica para encontrar bugs no codigo java. Diferente do PMD, ela usa bytecode Java compilado em vez do código-fonte. 

```
./gradlew findbugs
```

* [Checkstyle](http://checkstyle.sourceforge.net/): Essa ferramenta garante que o código segue [nossas guidelines de codigo Android](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md#2-code-guidelines). Olhe nosso [arquivo de configuração checkstyle](config/quality/checkstyle/checkstyle-config.xml). 

```
./gradlew checkstyle
```

### A Tarefa de check

Para garantir que seu codigo é valido e estavel use check:

```
./gradlew check
```

Isso irá executar as ferramentas de analise de codigos e testes unitarios na seguinte ordem:

![Check Diagram](images/check-task-diagram.png)
 
## Distribuição

O projeto pode ser distribuido usando [Crashlytics](http://support.crashlytics.com/knowledgebase/articles/388925-beta-distributions-with-gradle) ou [Google Play Store](https://github.com/Triple-T/gradle-play-publisher).

### Play Store

Nós usamos o plugin __Gradle Play Publisher__. Uma vez configurado corretamente, você será capaz de subir builds para os canais Alpha, Beta ou Production como queira.

```
./gradlew publishApkRelease
```

Leia [plugin documentation](https://github.com/Triple-T/gradle-play-publisher) para mais informações.

### Crashlytics

Você tambem pode usar Fabric's Crashlytics para distribuir versões betas. Lembre-se de adicionar suas informações da conta do Fabric em `app/src/fabric.properties`.

Para subir sua build release no Crashlytics execute:

```
./gradlew assembleRelease crashlyticsUploadDistributionRelease
```

## Iniciando Novo Projeto

Para iniciar rapidamente um novo projeto a partir desse boilerplate siga os seguintes passos:

* Baixe esse [repositorio como zip](https://github.com/ribot/android-boilerplate/archive/master.zip).
* Change the package name. 
* Altere o package name. 
  * Renomeie packages em main, androidTest e test usando Android Studio.
  * No arquivo `app/build.gradle`, `packageName` e `testInstrumentationRunner`.
  * Em `src/main/AndroidManifest.xml` e `src/debug/AndroidManifest.xml`.
* Crie um novo repositorio git, [veja esse tutorial de GitHub](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).
* Substitua o códido de exemplo com os códigos do seu app seguindo a mesma arquitetura.
* Em `app/build.gradle` adicione configurações de acesso para habilitar versoes release.
* Adicione a chaves API Key e secret em fabric.properties e descomente as configurações do plugin Fabric em `app/build.gradle`
* Atualize `proguard-rules.pro` para manter modelos (veja TODO no arquivo) e adicione regras extras ao arquivo se precisar.
* Atualize o arquivo README com as informções relevantes do novo projeto. 
* Atualize o arquivo LICENSE para corresponder às exigencias do novo projeto.

## License

```
    Copyright 2015 Ribot Ltd.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
```

