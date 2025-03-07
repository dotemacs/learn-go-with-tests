# Hello, World

**[Сав код за ово поглавље може да се нађе овде](https://github.com/quii/learn-go-with-tests/tree/main/hello-world)**

Традиционално је да први програм у новом језику буде [Hello, World (у преводу „Здраво свете“)](https://sr.wikipedia.org/sr-ec/Hello_World).

- Направи неки директоријум где ти је згодно
- Упиши нову датотеку са називом `hello.go` и упиши следећи код у њој:

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
}
```

Да би извршио/покренуо тај програм, укуцај `go run hello.go`.

## Опис рада

Када напишеш програм у Гоу, имаћете `main` пакет, који ће садржати функцију `main`. Пакети су начин да се групише Гоу код заједно.

Кључна реч `func` дефинише функцију, која је сачињена од имена и садржаја.

Са `import "fmt"` ми додајемо пакет који садржи `Println` функцију коју ми користимо да би исписали садржај на екрану.

## Како да тестирамо

Како се ово тестира? Пожељно је да се раздвоји код вашег "домена" са резултатом који он производи. `fmt.Println` је резултат \(исписивање садржаја\), док текст који ми проследимо је наш домен.

Да их раздвојимо да би било лакше за тестирање


```go
package main

import "fmt"

func Hello() string {
	return "Hello, world"
}

func main() {
	fmt.Println(Hello())
}
```

Направили смо нову функцију са `func`, али овај пут додали смо још једну кључну реч, `string`. То значи да ова функција враћа вредност чији је тип `string`.


Сада направи нову датотеку `hello_test.go` где ћемо да напишемо тест за нашу `Hello` функцију

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello()
	want := "Hello, world"

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

## Гоу модули?

Следећи корак је да покренемо тестове. Унеси `go test` у твој терминал. Ако тестови прођу, онда вероватно користите старију верзију Гоуа. Али ако користите Гоу верзију 1.16 или новију, тестови се неће покренути. Уместо тога, видећете грешку сличну овој у вашем терминалу:

```shell
$ go test
go: cannot find main module; see 'go help modules'
```

Шта је проблем? Укратко, [модули](https://blog.golang.org/go116-module-changes). Срећом, проблем се лако решава. Унесите `go mod init example.com/hello` у ваш терминал. То ће створити нову датотеку са следећим садржајем:

```
module example.com/hello

go 1.16
```

Ова датотека даје неопходне информације `go` алатки о вашем коду. Ако планирате да поделите вашу апликацију са другима, ви би уписали где је ваш код доступан за преузимање и информације, као и информације од којих других пакета ваш програм зависи. Име модула, example\.com\/hello, обично се односи на интернет адресу где модул може да се пронађе и преузме. Због компатибилности са алаткама које ћемо ускоро да почнемо да користимо, постарај се да име твог модула има тачку негде у свом имену, као тачку у .com у example\.com/hello. За сада, датотека твог модула је минимална и треба да остане таква. Да прочитате више о модулима, [погледајте званичне референце Гоу документације](https://golang.org/doc/modules/gomod-ref). Сада можемо да се вратимо тестирању и учењу Гоуа пошто ће тестови радити, чак и са Гоу верзијом 1.16.

У следећим поглављима, требаћете да извршите `go mod init SOMENAME` у сваком новом директоријуму пре него што извршите команде као `go test` или `go build`.

## Назад на Тестирање

Извршите `go test` у вашем терминалу. Тестови би требали да прођу. Само да проверите, пробајте намерно да покварите тестове, измењивањем променљиве `want`.

Видели сте у горњем примеру да нисте требали да бирате између разних библиотека за тестирање и да студирате њихова упутства за инсталирање. Све што вам је било потребно, уграђено је у сам језик, чак је и синтакса иста као остали код који сте писали.

### Писање тестова

Писање теста је исто као и писање функције, уз неколико правила

* Треба да буде у датотеци са именом као `xxx_test.go`
* Тест функција мора да почиње са речију `Test`
* Тест функција има само један аргумент `t *testing.T`
* Да би користили `*testing.T` тип, треба да додате `import "testing"`, као што смо урадили са `fmt` у другој датотеци

За сада, довољно је да знате да ваш `t` од типа `*testing.T` је начин да "уђете" у тест библиотеку са којом можете да радите ствари попут `t.Fail()` када желите да ваш тест не прође.

Обрадили смо неке нове теме:

#### `if`
If исказ у Гоу је сличан као у другим програмерским језицима.

#### Декларисање променљивих

Неке променљиве декларишемо са синтаксом `varName := value`, што нам омогућава да поново користимо неке вредности у нашем тесту ради читљивости.

#### `t.Errorf`

Ми позивамо `Errorf` _методу_ на наш `t`, који ће приказати поруку и пасти тест. Ово `f` (на крају методе `Errorf`) означава формат, који нам омогућава да направимо вредност која може бити уметнута у `%q` (која служи да "чува место" за променљиву). Када тест не прође, треба да буде јасно како ради.

Можете да прочитате о томе како се променљиве са тексто могу обликовати у [fmt документацији](https://pkg.go.dev/fmt#hdr-Printing). За тестове, `%q` је врло корисна јер ставља вредности у двоструке наводнике.

Касније ћемо објаснити разлику између метода и функција.

### Гоува документација

Још једна квалитетна карактеристика Гоуа је документација. Управо смо видели документацију за fmt пакет на званичној веб страници за преглед пакета, а Гоu такође пружа начине за брзо приступање документацији чак и када нисти прикључени на интернет.

Гоу има уграђену алатку, doc, која омогућава да се прегледају пакети који су инсталирани на вашем систему, или модул на коме тренутно радите. Да погледате неку документацију за Printing:

```
$ go doc fmt
package fmt // import "fmt"

Package fmt implements formatted I/O with functions analogous to C's printf and
scanf. The format 'verbs' are derived from C's but are simpler.

# Printing

The verbs:

General:

    %v	the value in a default format
    	when printing structs, the plus flag (%+v) adds field names
    %#v	a Go-syntax representation of the value
    %T	a Go-syntax representation of the type of the value
    %%	a literal percent sign; consumes no value
...
```

Гоува друга алатка за прегледање документације је pkgsite команда, која стоји иза Гоуве знваниче веб странице за преглед пакета. Можете да инсталирате pkgsite са `go install golang.org/x/pkgsite/cmd/pkgsite@latest`, а онда је покрените са `pkgsite -open .`. Гоува install команда ће скинути изворни код и направиће извршни програм. За уобичајену инсталацију Гоуа, програм ће бити у `$HOME/go/bin` за Линукс и Мек ОС, и `%USERPROFILE%\go\bin` за Виндоус. Ако већ нисте додали ове смернице вашем $PATH варијабли окружења, било би добро да то сада урадите зашто што ће олакшати коришћење алатки инсталираних од стране Гоуа.

Велика већина стандардне библиотеке има одличну документацију са примерима. Било би добро да погледате [http://localhost:8080/testing](http://localhost:8080/testing) шта све имате на приступ.


### Hello, YOU

Сада када имамо тест, можемо безбедно да наставимо на даљем раду на нашем софтверу.

У задњем примеру, ми смо писали тест _после_ кода да би моглид а видите пример како се пише тест и функција. Од сада па на даље, _прво ћемо писати тестове_.

Следећи задатак је да наведемо примаоца поздрава.

Да почнемо тако што ћемо прво да додамо тај захтев у тесту. Ово је основно програмирање засновано на тестовима (test-driven development) које нам омогућава да _заправо_ тестирамо оно што желимо. Када пишете тестове после самог програма/кода који тестирате, постоји ризик да ваши тестови наставе да пролазе чак и када код ради како је намењено.

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello("Chris")
	want := "Hello, Chris"

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

Покрени `go test`, и имаћеш грешку у компајлирању

```text
./hello_test.go:6:18: too many arguments in call to Hello
    have (string)
    want ()
```

Када користите статички типован језик као Гоу, важно је да _слушате компајлер_. Компајлер разуме како ваш код треба да се сложи заједно да би радио, уместо вас.

У овом случају компајлер вам говори шта вам треба да би наставили. Морамо да изменимо функцију `Hello` да прихвати аргумент.

Уредите функцију `Hello` да прихвати аргумент типа стринг

```go
func Hello(name string) string {
	return "Hello, world"
}
```

Ако покренете ваше тестове `hello.go` неће да се изгради зато што нисте додали аргумент. Додајте реч "world" да би то омогућили.

```go
func main() {
	fmt.Println(Hello("world"))
}
```

Сада када покренете ваше тестове, треба да видите нешко као

```text
hello_test.go:10: got 'Hello, world' want 'Hello, Chris''
```

Ми наизад имамо програм који се изграђује али се не подудара за захтевима који су уписани у тесту.

Хајде да учинимо да тест прође коришћењем имена као аргумент, који се спаја са `Hello,`

```go
func Hello(name string) string {
	return "Hello, " + name
}
```

Када покренете тестове, они би требали да прођу. Обично, као део програмирања засновано на тестовима (test driven development или скраћено TDD), сада би требало да _[рефакторишемо код](https://sr.wikipedia.org/wiki/Рефакторисање_кода)_.

### Белешка о контроли изворног кода

У овом тренутку, ако користите контролу изворног кода \(што би требали\), ја бих сачувао тренутни код. Имамо програм који ради подржан тестом.

Ја још не би поделио ову верзију кода са другима, зато што планирам да га рефакторишем. Добро би било да се сачува код у овом стању у случају да се направи нека грешка са рефакторисањем, јер увек можеш да се вратимо на верзију кода која ради.

Овде нема много тога за рефакторисање, али можемо да уведемо још једну језичку функцију, _константе_.

### Константе

Константе се дефинишу овако

```go
const englishHelloPrefix = "Hello, "
```

Сада можемо да рефакторишемо наш код

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
	return englishHelloPrefix + name
}
```

После рефакторисања, поново покрени тестове да би се уверили да нисмо нешто покварили.

Вреди размислити о употреби константи да би се обележило значење вредности, а понекад и да би се помогао учинак.

## Hello, world... поново

Следећи захтев је да када се наша функција користи са празним аргументом типа стринг, да она подразумевано испише "Hello, World", уместо "Hello, ".

Почнимо писањем теста који неће проћи

```go
func TestHello(t *testing.T) {
	t.Run("saying hello to people", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hello, Chris"

		if got != want {
			t.Errorf("got %q want %q", got, want)
		}
	})
	t.Run("say 'Hello, World' when an empty string is supplied", func(t *testing.T) {
		got := Hello("")
		want := "Hello, World"

		if got != want {
			t.Errorf("got %q want %q", got, want)
		}
	})
}
```

Ово уводимо нову алатку у нашем аресеналу за тестирање: под тестове. Некада је корисно да се тестови групишу око нечега и да имаш под тестове који описују различите сценарије.

Предност овог приступа је да можеш да подесиш код који може да се искористи и у другим тестовима.

Док имамо тест који не пролази, хајде да поправимо код, користећи `if`.

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
	if name == "" {
		name = "World"
	}
	return englishHelloPrefix + name
}
```

Ако покренемо наше тестове требали би да видимо да ли подржава наше нове захтеве и да нисмо случајно нешто покварили.

Важно је да су ваши тестови _чиста спецификација_ која се очекује од кода. Али има и кода који се понавља када проверимо да ли је исход оно што очекујемо.

Рефакторисање није _само_ за код који је за продукцију(крају употребу).

Сада када тестови пролазе, требали би да рефакторишемо наше тестове.

```go
func TestHello(t *testing.T) {
	t.Run("saying hello to people", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hello, Chris"
		assertCorrectMessage(t, got, want)
	})

	t.Run("empty string defaults to 'world'", func(t *testing.T) {
		got := Hello("")
		want := "Hello, World"
		assertCorrectMessage(t, got, want)
	})

}

func assertCorrectMessage(t testing.TB, got, want string) {
	t.Helper()
	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

Шта смо урадили?

We've refactored our assertion into a new function. This reduces duplication and improves the readability of our tests. We need to pass in `t *testing.T` so that we can tell the test code to fail when we need to.

For helper functions, it's a good idea to accept a `testing.TB` which is an interface that `*testing.T` and `*testing.B` both satisfy, so you can call helper functions from a test, or a benchmark (don't worry if words like "interface" mean nothing to you right now, it will be covered later).

`t.Helper()` is needed to tell the test suite that this method is a helper. By doing this, when it fails, the line number reported will be in our _function call_ rather than inside our test helper. This will help other developers track down problems more easily. If you still don't understand, comment it out, make a test fail and observe the test output. Comments in Go are a great way to add additional information to your code, or in this case, a quick way to tell the compiler to ignore a line. You can comment out the `t.Helper()` code by adding two forward slashes `//` at the beginning of the line. You should see that line turn grey or change to another color than the rest of your code to indicate it's now commented out.

When you have more than one argument of the same type \(in our case two strings\) rather than having `(got string, want string)` you can shorten it to `(got, want string)`.

### Back to source control

Now that we are happy with the code, I would amend the previous commit so that we only check in the lovely version of our code with its test.

### Discipline

Let's go over the cycle again

* Write a test
* Make the compiler pass
* Run the test, see that it fails and check the error message is meaningful
* Write enough code to make the test pass
* Refactor

On the face of it this may seem tedious but sticking to the feedback loop is important.

Not only does it ensure that you have _relevant tests_, it helps ensure _you design good software_ by refactoring with the safety of tests.

Seeing the test fail is an important check because it also lets you see what the error message looks like. As a developer it can be very hard to work with a codebase when failing tests do not give a clear idea as to what the problem is.

By ensuring your tests are _fast_ and setting up your tools so that running tests is simple you can get in to a state of flow when writing your code.

By not writing tests, you are committing to manually checking your code by running your software, which breaks your state of flow. You won't be saving yourself any time, especially in the long run.

## Keep going! More requirements

Goodness me, we have more requirements. We now need to support a second parameter, specifying the language of the greeting. If a language is passed in that we do not recognise, just default to English.

We should be confident that we can easily use TDD to flesh out this functionality!

Write a test for a user passing in Spanish. Add it to the existing suite.

```go
	t.Run("in Spanish", func(t *testing.T) {
		got := Hello("Elodie", "Spanish")
		want := "Hola, Elodie"
		assertCorrectMessage(t, got, want)
	})
```

Remember not to cheat! _Test first_. When you try to run the test, the compiler _should_ complain because you are calling `Hello` with two arguments rather than one.

```text
./hello_test.go:27:19: too many arguments in call to Hello
    have (string, string)
    want (string)
```

Fix the compilation problems by adding another string argument to `Hello`

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}
	return englishHelloPrefix + name
}
```

When you try and run the test again it will complain about not passing through enough arguments to `Hello` in your other tests and in `hello.go`

```text
./hello.go:15:19: not enough arguments in call to Hello
    have (string)
    want (string, string)
```

Fix them by passing through empty strings. Now all your tests should compile _and_ pass, apart from our new scenario

```text
hello_test.go:29: got 'Hello, Elodie' want 'Hola, Elodie'
```

We can use `if` here to check the language is equal to "Spanish" and if so change the message

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == "Spanish" {
		return "Hola, " + name
	}
	return englishHelloPrefix + name
}
```

The tests should now pass.

Now it is time to _refactor_. You should see some problems in the code, "magic" strings, some of which are repeated. Try and refactor it yourself, with every change make sure you re-run the tests to make sure your refactoring isn't breaking anything.

```go
	const spanish = "Spanish"
	const englishHelloPrefix = "Hello, "
	const spanishHelloPrefix = "Hola, "

	func Hello(name string, language string) string {
		if name == "" {
			name = "World"
		}

		if language == spanish {
			return spanishHelloPrefix + name
		}
		return englishHelloPrefix + name
	}
```

### French

* Write a test asserting that if you pass in `"French"` you get `"Bonjour, "`
* See it fail, check the error message is easy to read
* Do the smallest reasonable change in the code

You may have written something that looks roughly like this

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == spanish {
		return spanishHelloPrefix + name
	}
	if language == french {
		return frenchHelloPrefix + name
	}
	return englishHelloPrefix + name
}
```

## `switch`

When you have lots of `if` statements checking a particular value it is common to use a `switch` statement instead. We can use `switch` to refactor the code to make it easier to read and more extensible if we wish to add more language support later

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	prefix := englishHelloPrefix

	switch language {
	case spanish:
		prefix = spanishHelloPrefix
	case french:
		prefix = frenchHelloPrefix
	}

	return prefix + name
}
```

Write a test to now include a greeting in the language of your choice and you should see how simple it is to extend our _amazing_ function.

### one...last...refactor?

You could argue that maybe our function is getting a little big. The simplest refactor for this would be to extract out some functionality into another function.

```go

const (
	spanish = "Spanish"
	french  = "French"

	englishHelloPrefix = "Hello, "
	spanishHelloPrefix = "Hola, "
	frenchHelloPrefix  = "Bonjour, "
)

func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	return greetingPrefix(language) + name
}

func greetingPrefix(language string) (prefix string) {
	switch language {
	case french:
		prefix = frenchHelloPrefix
	case spanish:
		prefix = spanishHelloPrefix
	default:
		prefix = englishHelloPrefix
	}
	return
}
```

A few new concepts:

* In our function signature we have made a _named return value_ `(prefix string)`.
* This will create a variable called `prefix` in your function.
  * It will be assigned the "zero" value. This depends on the type, for example `int`s are 0 and for `string`s it is `""`.
    * You can return whatever it's set to by just calling `return` rather than `return prefix`.
  * This will display in the Go Doc for your function so it can make the intent of your code clearer.
* `default` in the switch case will be branched to if none of the other `case` statements match.
* The function name starts with a lowercase letter. In Go, public functions start with a capital letter, and private ones start with a lowercase letter. We don't want the internals of our algorithm exposed to the world, so we made this function private.
* Also, we can group constants in a block instead of declaring them on their own line. For readability, it's a good idea to use a line between sets of related constants.

## Wrapping up

Who knew you could get so much out of `Hello, world`?

By now you should have some understanding of:

### Some of Go's syntax around

* Writing tests
* Declaring functions, with arguments and return types
* `if`, `const` and `switch`
* Declaring variables and constants

### The TDD process and _why_ the steps are important

* _Write a failing test and see it fail_ so we know we have written a _relevant_ test for our requirements and seen that it produces an _easy to understand description of the failure_
* Writing the smallest amount of code to make it pass so we know we have working software
* _Then_ refactor, backed with the safety of our tests to ensure we have well-crafted code that is easy to work with

In our case, we've gone from `Hello()` to `Hello("name")` and then to `Hello("name", "French")` in small, easy-to-understand steps.

Of course, this is trivial compared to "real-world" software, but the principles still stand. TDD is a skill that needs practice to develop, but by breaking problems down into smaller components that you can test, you will have a much easier time writing software.
