# Инсталирај Гоу (Go), подеси окружење за рад


Званична упутства за гоу су доступна [овде](https://golang.org/doc/install).

## Гоу Окружење

### Гоу Модули

Гоу 1.11 је додао [Модуле](https://go.dev/wiki/Modules). То је уобичајени начин да се користи/компајлира Гоу од верзије 1.16 па на даље. Што значи да је употреба `GOPATH` (варијабла за окружење, која се користила у ранијим верзијама Гоу, за подешавање Гоу језика) није препоручљиво.

Модули покушавају да реше проблеме везане за употребу других, екстерних библиотека (које се зову, у Гоу екосистему, у буквалном преводу пакети: packages), њихових разних верзија, тако што омогућавају да се изграде идентични програми сваки пут када се код искомпајлира/изгради. Они такође омогућавају да корисници користе Гоу код ван `GOPATH` варијабле за окружење.

Употреба модула је једноставана. Изабери неку директоријум ван `GOPATH` варијабле, и направи нови модул са командом: `go mod init`.

Датотека `go.mod` ће бити исписана, која ће да садржи локацију модула, Гоу верзију и друге модули од који су потребни да би се програм успешно изградио.

Ако `<modulepath>` није наведен, `go mod init` ће покушати да погоди локацију за модул на основу структуре директоријума. Локација може да буде локација по избору, ако се да као аргумент.

```sh
mkdir my-project
cd my-project
go mod init <modulepath>
```

`go.mod` датотека може да изгледа овако:

```
module cmd

go 1.16

```

Команде укључују и њихову документацију која образлаже све присутне `go mod` команде.

```sh
go help mod
go help mod init
```

## Статична анализа Гоу кода (Linting)

Побољшана команда од за статичну анализу Гоу кода, од оне која већ долази уз Гоу, може да се подеси преко [GolangCI-Lint](https://golangci-lint.run).

Може да се инсталира на следећи начин:

```sh
brew install golangci-lint
```

## Рефакторисање и ваше алатке

Велики нагласак ове књиге је важност рефакторисања.

Ваше алатке вам могу помоћи да са извршите већи рефакторинг са самопоуздањем.

Требало би да будете довољно упознати са вашим едитором/окружењем за програмирање, да би могли да извршите следеће помоћу једноставних комбинација на тастеру:

- **Extract/Inline variable - (??? променљива)**. Taking magic values and giving them a name lets you simplify your code quickly.
- **Extract method/function**. It is vital to be able to take a section of code and extract functions/methods
- **Rename**. You should be able to rename symbols across files confidently.
- **go fmt**. Go has an opinioned formatter called `go fmt`. Your editor should run this on every file saved.
- **Run tests**. You should be able to do any of the above and then quickly re-run your tests to ensure your refactoring hasn't broken anything.

In addition, to help you work with your code, you should be able to:

- **View function signature**. You should never be unsure how to call a function in Go. Your IDE should describe a function in terms of its documentation, its parameters and what it returns.
- **View function definition**. If it's still unclear what a function does, you should be able to jump to the source code and try and figure it out yourself.
- **Find usages of a symbol**. Understanding a function's context can help you make decisions when refactoring.

Mastering your tools will help you concentrate on the code and reduce context switching.

## Wrapping up

At this point, you should have Go installed, an editor available, and some basic tooling in place. Go has a very large ecosystem of third-party products. We have identified a few useful components here. For a more complete list, see [https://awesome-go.com](https://awesome-go.com).
