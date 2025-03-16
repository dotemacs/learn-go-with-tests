# Зашто користити модуларно тестирање (unit tests) и како може да вам помогне

[Овде је видео снимак са мојим обашњењем](https://www.youtube.com/watch?v=Kwtit8ZEK7U)

Ако нисте за видео снимке, ево вам текст верзије.

## Софтвер

Идеја софтвера је да променљив. Зато се зове _soft_ ware (ware, роба, мекана роба), која може да се прилагођава у поређењу са хардвером. Сјајан инжењерски тим треба да је невероватна предност за компанију, који могу да пишу системе који евулуирају са пословањем и даље пружају на значају.

Али зашто смо лоши га правимо? За колико пројеката сте чули који су пропали? Или су постали "стари" и морају бити потпуно поново написани (а поновно писање често не успе!).

Како софтверски систем "не успе"? Зар не може да се промени док не успе? То је оно што нам је обећано!

Пуно људи бира Гоу да са њиме пише систем зато што има број опција које се надамо да ће га учинити отпорнијим да не застаре.

- У поређењу са мојим предходним животом као Скала програмер где [ја описујем како ти је у њему пружено уже да се обесиш](http://www.quii.dev/Scala_-_Just_enough_rope_to_hang_yourself), Гоу има само 25 кључних речи и _пуно_ система могу бити напсани само са његовом стандардном и неколико малих додатних библиотека. Нада је да са Гоуом можеш да напишеш код и када се вратиш њему после шест месеци и даље ће имати смисла.
- Алати у погледу тестирања, стандард успешности, провера синтаксе и софтверско распоређивање су првокласни у поређењу са већином алтернатива.
- Стандардна библиотека је сјајна.
- Веома велика брзина компилације за где брзо добијате повратне информације о вашем коду
- Обећање компатибилности уназад. Изгледа да ће Гоу добити generics (начин да се пише код који може да ради са разним типовима) и друге карактеристике, у будућности, али дизајнери су обећали да ће чак и Гоу код који сте написали пре пет година и даље моћи да се компајлира. Ја сам буквално провео више недеља надограђујући пројекат са Скале 2.8 на 2.10.

Чак и са свим тим великим својствима и даље можемо да правимо страшно
лоше системе, тако да би требало да погледамо прошлост и разумемо
лекције у софтверском инжењерингу која се примењују без обзира колико
сматрате да је сјајан ваш програмски језик.

1974. године паметни софтверски инжењер [Мани Лиман](https://en.wikipedia.org/wiki/Manny_Lehman_%28computer_scientist%29) написао је [Лиманов закон еволуције софтвера](https://en.wikipedia.org/wiki/Lehman%27s_laws_of_software_evolution).


> Закони описују равнотежу између снага које воде нова дешавања с
> једне стране, а снаге које успоравају напредак с друге стране.

Ове је важно да се разуме да не би живели у безнадежном стању где
пишемо систем који постану старомодни и који се опет мора да се пишу
као замена.


## Закон непрестане промене

> Било који софтверски систем који се озбиљно користи мора да се
> промени или постане све мање користан

Очигледно је да је систем _мора_ да се промени или да постане мање
користан, али колико често се то игнорише?

Многи тимови су подстакнути да доставе пројекат на одређени датум, а
затим прелазе на следећи пројекат. Ако је софтвер "сретан", постоји
барем неки процес где се пројекат да другом тиму да га одржава, који
га нису написали.

Људи се често труде на нађу начин који ће им помоћи да се пројекат
"испоручи брзо", али не фокусирају се на дуговечност система у смислу
како он треба да се развија даље.

Чак и ако сте изванредан софтвера инжењер, ипак нећете знати за будуће
потребе вашег система. Како се пословни захтеви мењају, сјајни код
који ви будете написали престаће да буде релевантан.

Лиману је баш пошло 1970их зато што нам је он дао још један закон за
размишљање.

## Закон о повећању сложености

> Као се систем развија, његова сложеност расте, осим ако се не
> постарате да је смањите

Оно што овде каже је да не можемо имати софтверске тимове који слепо
пишу нову функционалност, гомилајући их у софтверу у нади да ће оно
дугорочно преживети.

Ми **морамо** да се старамо о комплексности систем док се знање о
нашем домену мења.

## Рефакторисање

Има много аспеката у софтвер инжењерингу који чини софтвер прилагодљивим, као:

- Оснаживање програмера
- Генерално „добар“ код. Разумно раздвајање одговорности, итд.
- Комуникационе вештине
- Архитектура
- Погодност за обсервацију
- Могућност за распоређивање
- Аутоматизовани тестови
- Петље повратних информација

Фокусираћу се на рефакторисање. То је фраза која се много користи
"морамо да рефакторишемо ово" - каже се програмеру на почетку првог
дана на новом послу без много разматрања.

Одакле долази ова фраза? Како је рефакторисање другачије од писања кода?

Ја знам да смо многи дуриги и ја _мислили_ да ми рефакторишемо али смо били у заблуди.

[Мартин Фаулер објашњава како су ту људи погрешили](https://martinfowler.com/bliki/RefactoringMalapropism.html)

> Међутим, термин "рефакторисање" се често користи када није
> прикладно. Ако неко говори о систему који не ради неколико дана, док
> га они рефакторишу, можете бити прилично сигурни да га они не
> рефакторишу.

Па шта је онда?

### Факторизација

У школи сте вероватно учили о факторизацији. Ево једноставног примера

Израчунај `1/2 + 1/4`

Да би то урадили ви _факторишете_ именилац, претварајући израз у

`2/4 + 1/4` из кога изведете `3/4`.

Из овога можемо да извучемо поуку. Када ми _факторишемо израз_ ми *нисмо променили значење израза*. Оба су једнака `3/4` али ми смо их само прилагодили да нам буду лакши за рад, тако што смо променили `1/2` у `2/4` он се лакше уклопио у наш "домен".

Када рефакторишете свој код, ви покушавате да нађете начин да учините ваш код лакшим за разумевање и да се "уклопи" у ваше тренутно разумевање шта систем треба да ради. Кљчно је **да ви не требате да му мењате опхођење**.

#### Пример у Гоу

Ево функције која поздравља `name` (име) на одређеном `language` (језику)

    func Hello(name, language string) string {

      if language == "es" {
         return "Hola, " + name
      }

      if language == "fr" {
         return "Bonjour, " + name
      }

      // imagine dozens more languages

      return "Hello, " + name
    }

Имати десетине `if` израза не делује добро, а такође имамо дуплирање код спајања поздрава специфичног за језик са `,` и `name`. Зато ћу рефакторисати код.

    func Hello(name, language string) string {
      	return fmt.Sprintf(
      		"%s, %s",
      		greeting(language),
      		name,
      	)
    }

    var greetings = map[string]string {
      "es": "Hola",
      "fr": "Bonjour",
      //etc..
    }

    func greeting(language string) string {
      greeting, exists := greetings[language]

      if exists {
         return greeting
      }

      return "Hello"
    }

Природа овог рефакторисања није битна, важно је да нисам променио понашање кода.

Када рефакторишете, можете радити шта год желите – додавати интерфејсе, нове типове, функције, методе итд. Једино правило је да не мењате опхођење кода.

### Када рефакторишете код, не смете мењати понашање/опхђење кода

Ово је веома важно. Ако истовремено мењате понашање, ви истовремено
радите _два_ стварно различита посла. Као софтверски инжењери, учимо
да поделимо системе на различите датотеке/пакете/функције/итд јер
знамо да је тешко разумети велику гомилу нечега.

Не желимо да размишљамо о пуно ствари одједном, јер тада правимо
грешке. Видео сам много покушаја рефакторисања који су пропали зато
што су програмери узели више него што могу да поднесу.

Када сам радио факторизације на часовима математике са оловком и
папиром, морао сам ручно да проверим да нисам променио значење израза
у својој глави. Како знамо да не мењамо понашање када рефакторишемо
код, посебно у систему који није тривијалан?

Они који одлуче да не пишу тестове обично ће зависити од ручног
тестирања. За било шта осим малог пројекта, ово ће бити огроман
губитак времена и неће бити одрживо на дуги рок.

**Да бисте сигурно рефакторисали, потребни су вам модуларни тестови**
јер они пружају:

- Могућност да са поуздањем можете променити код без бриге да ћете променити понашање
- Документацију за људе о томе како систем треба да се понаша
- Много брже и поузданије повратне информације него ручно тестирање

#### Пример у Гоу

Модуларни тест за нашу `Hello` функцију могао би изгледати овако

    func TestHello(t *testing.T) {
      got := Hello(“Chris”, es)
      want := "Hola, Chris"

      if got != want {
         t.Errorf("got %q want %q", got, want)
      }
    }

На командној линији могу да покренем `go test` и добијем тренутне повратне информације о томе да ли су моји напори у рефакторисању променили понашање. У пракси је најбоље имати комбинацију на тастеру за покретање тестова унутар вашег едитора/окружења за програмирање.

Желите да дођете у стање где радите

- Мало рефакторисање
- Покренете тестове
- Поновите

Све у оквиру веома кратког повратног круга како не бисте залазили у замке и правили грешке.

Имати пројекат у ком су сви ваши кључни поступци модуларно тестирани и
дају вам повратне информације за мање од секунде је веома оснажујућа
сигурносна мрежа за смело рефакторисање када је то потребно. Ово нам
помаже да савладамо комплексности коју Леман описује.

## Ако су модуларни тестови толико одлични, зашто понекад постоји отпор према њиховом писању?

Са једне стране имате људе (као што сам ја) који кажу да су модуларни
тестови важни за дугорочно здравље вашег система јер осигуравају да
можете наставити са рефакторисањем са поверењем.

Са друге стране имате људе који описују искуства где модуларни тестови
заправо _онемогућавају_ рефакторисање.

Питајте се, колико често морате да мењате своје тестове када
рефакторишете? Током година био сам на многим пројектима са врло
добрим покрићем кода тестовима и ипак су инжењери оклевали да
рефакторишу због мишљења да ће њихова измена захтевати напор.

Ово је супротно од онога што нам је обећано!

### Why is this happening?

Imagine you were asked to develop a square and we thought the best way to accomplish that would be stick two triangles together. 

![Two right-angled triangles to form a square](https://i.imgur.com/ela7SVf.jpg)

We write our unit tests around our square to make sure the sides are equal and then we write some tests around our triangles. We want to make sure our triangles render correctly so we assert that the angles sum up to 180 degrees, perhaps check we make 2 of them, etc etc. Test coverage is really important and writing these tests is pretty easy so why not? 

A few weeks later The Law of Continuous Change strikes our system and a new developer makes some changes. She now believes it would be better if squares were formed with 2 rectangles instead of 2 triangles. 

![Two rectangles to form a square](https://i.imgur.com/1G6rYqD.jpg)

She tries to do this refactor and gets mixed signals from a number of failing tests. Has she actually broken important behaviours here? She now has to dig through these triangle tests and try and understand what's going on. 

_It's not actually important that the square was formed out of triangles_ but **our tests have falsely elevated the importance of our implementation details**. 

## Favour testing behaviour rather than implementation detail

When I hear people complaining about unit tests it is often because the tests are at the wrong abstraction level. They're testing implementation details, overly spying on collaborators and mocking too much. 

I believe it stems from a misunderstanding of what unit tests are and chasing vanity metrics (test coverage). 

If I am saying just test behaviour, should we not just only write system/black-box tests? These kind of tests do have lots of value in terms of verifying key user journeys but they are typically expensive to write and slow to run. For that reason they're not too helpful for _refactoring_ because the feedback loop is slow. In addition black box tests don't tend to help you very much with root causes compared to unit tests. 

So what _is_ the right abstraction level?

## Writing effective unit tests is a design problem

Forgetting about tests for a moment, it is desirable to have within your system self-contained, decoupled "units" centered around key concepts in your domain. 

I like to imagine these units as simple Lego bricks which have coherent APIs that I can combine with other bricks to make bigger systems. Underneath these APIs there could be dozens of things (types, functions et al) collaborating to make them work how they need to.

For instance if you were writing a bank in Go, you might have an "account" package. It will present an API that does not leak implementation detail and is easy to integrate with.

If you have these units that follow these properties you can write unit tests against their public APIs. _By definition_ these tests can only be testing useful behaviour. Underneath these units I am free to refactor the implementation as much as I need to and the tests for the most part should not get in the way.

### Are these unit tests?

**YES**. Unit tests are against "units" like I described. They were _never_ about only being against a single class/function/whatever.

## Bringing these concepts together

We've covered

- Refactoring
- Unit tests
- Unit design

What we can start to see is that these facets of software design reinforce each other. 

### Refactoring

- Gives us signals about our unit tests. If we have to do manual checks, we need more tests. If tests are wrongly failing then our tests are at the wrong abstraction level (or have no value and should be deleted).
- Helps us handle the complexities within and between our units.

### Unit tests

- Give a safety net to refactor.
- Verify and document the behaviour of our units.

### (Well designed) units

- Easy to write _meaningful_ unit tests.
- Easy to refactor.

Is there a process to help us arrive at a point where we can constantly refactor our code to manage complexity and keep our systems malleable?

## Why Test Driven Development (TDD)

Some people might take Lehman's quotes about how software has to change and overthink elaborate designs, wasting lots of time upfront trying to create the "perfect" extensible system and end up getting it wrong and going nowhere. 

This is the bad old days of software where an analyst team would spend 6 months writing a requirements document and an architect team would spend another 6 months coming up with a design and a few years later the whole project fails.

I say bad old days but this still happens! 

Agile teaches us that we need to work iteratively, starting small and evolving the software so that we get fast feedback on the design of our software and how it works with real users;  TDD enforces this approach.

TDD addresses the laws that Lehman talks about and other lessons hard learned through history by encouraging a methodology of constantly refactoring and delivering iteratively.

### Small steps

- Write a small test for a small amount of desired behaviour
- Check the test fails with a clear error (red)
- Write the minimal amount of code to make the test pass (green)
- Refactor
- Repeat

As you become proficient, this way of working will become natural and fast.

You'll come to expect this feedback loop to not take very long and feel uneasy if you're in a state where the system isn't "green" because it indicates you may be down a rabbit hole. 

You'll always be driving small & useful functionality comfortably backed by the feedback from your tests.

## Wrapping up 

- The strength of software is that we can change it. _Most_ software will require change over time in unpredictable ways; but don't try and over-engineer because it's too hard to predict the future.
- Instead we need to make it so we can keep our software malleable. In order to change software we have to refactor it as it evolves or it will turn into a mess
- A good test suite can help you refactor quicker and in a less stressful manner
- Writing good unit tests is a design problem so think about structuring your code so you have meaningful units that you can integrate together like Lego bricks.
- TDD can help and force you to design well factored software iteratively, backed by tests to help future work as it arrives.
