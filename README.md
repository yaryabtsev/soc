### Анализ "искусственных собеседников"

У большинства голосовых помощников и чат-ботов отсутствует "память" (буфер), хотя даже калькуляторы умеют запоминать 
несколько предыдущих операций.

* Алиса от Яндекса пытается интерпретировать только последнюю реплику, а, без контекста, последующее общение
превращается в фарс.
* В отличие от Алисы, [Демо "Блондинку"](https://nanosemantics.ai/demo/blonde-bot/) слегка красит ее "короткая память",
это делает ее ветреной, ~~а Алису - бесячей~~.
* pBot`ы умеют обрабатывать разговорную лексику, но больше ничего не умеют.
![pBot](1.jpeg)
* Нашел на [гитхабе](https://github.com/Koziev/chatbot) пару адекватных реализаций чат-ботов. В одной из первых версий(Релиз 0.0.2.11),
этот бот уже имел интерпретировать вопросы, связанные с обращением к контексту (там есть пример, в котором первым 
вопросом у бота спросили: "какой сегодня день недели?", после этого задали ряд вопросов: "а вчера какой был?", "ну а завтра какой будет?"... -
бот справился. такой функционал делает его в каком-то смысле текстовым помощником-ассистентом). То факт, что буфер был
уже в первой версии проекта, дает ему преимущество.
 

### Эксперимент

Для чистоты эксперимента я использовал функционал Вконтакте для преобразования коротких голосовых сообщений в текст, т.к.
бот на вход получает текст. Также воспользовался функционалом javascript для озвучивания выходного текста. (Если вк-api
откатят до приемлемой версии, можно будет сделать полноценный голосовой помощник). 

Текст переписки:
```
B:> Привет, рада буду пообщаться
H:> привет
```
Имя бота - Вика, это объясняет окончания...
```
B:> если не секрет, как твое имя?
H:> ярик.

B:> как поживаешь, ярослав ?
H:> нармуль.

B:> не мертвый груз, а груз
B:> подскажи, пожалуйста, сколько тебе лет?
H:> двадцать.

B:> у тебя есть семья?
H:> Родители.

B:> расскажи, как относишься к группе Ночные Снайперы?
H:> Мне нравятся многие их песни, хотя некоторые припевы кажутся слабыми и поднадоели.

B:> а какие песни нравятся тебе?
H:> Ну, я фанат панк-рока, из русского скорее Гражданская оборона.

B:> любишь ли ты песни Эминема?
H:> Нет, я пытался осилить фильм про него. Сдался.

B:> как ты его пыталась осилить?
H:> Фильм? Ну я посмотрел минут сорок. Кстати, я - парень. Ярослав.
```
Там есть файл с конфигурациями и я его забыл поменять, поэтому меня проинтерпретировали, как существо женского пола. 
Это также говорит о неспособности интерпретирования чужих имен или это баг, т.к. полная форма имени определена корректно
и этого должно быть достаточно.
```
B:> очень приятно, ты кто по профессии?
H:> Я студент, учусь на факультете компьютерных наук.

B:> о чем ты вообще любишь болтать?
H:> О фильмах. Я смотрел много фильмов и сериалов.

B:> а какие фильмы ты любишь?
H:> Оу, наверное комедии, их я смотрел реально много. Хорошо разбираюсь в слэшерах. Ненавижу фильмы про мафию, кроме 
Бэтмена.

B:> почему ты их не любишь?
H:> Ну, эти фильмы сосредотечены вокруг власти, главные герои делят територию и жертвуют своими шестерками ради 
получения большего влияния, власти. Мне же, больше нравится наблюдать за тем, как эти "шестерки" чего-то добиваются 
в жизни.
```
Тут произошла первая ошибка, скорее всего это связанно с неправильной настройкой видеокарты и отсутствие 
предустановленных драйверов на моем ubuntu. Но общение продолжилось, ошибка была не фатальной:
```
2021-11-26 16:09:54 ERROR   SimpleAnsweringMachine    Two consequent questions issued by bot test_bot: prev="ты занимаешься каким-нибудь спортом?", new="кого бы ты назвал лучшим актером кино?"
2021-11-26 16:10:19 ERROR   SimpleAnsweringMachine    more than 2 utterances issued by bot: prev="🤢", new="ты делал прививку от COVID-19?"
2021-11-26 16:10:19 ERROR   SimpleAnsweringMachine    SAY: repetition detected in bot test_bot, phrase "ты делал прививку от COVID-19?" is muted
```
```
B:> ты любишь наблюдать за тем, как эти " шестерки " работают
B:> 🤢
H:> Да, конечно, смотрел клерков? Крутой фильм!
```
Я забыл, что Вика - девочка и попытался проявить инициативу, не только отвечать на вопросы, но и задать парочку. Тот 
факт, что я забыл, что общаюсь с Викой слегка оправдывает ее первую ошибку с неправильным определением моего пола.
```
B:> ты любишь бананы?
H:> Да, но они закончились, а в магазин идти лень.
```
Резкое изменение темы!!!
```
B:> лучше кашки не доложь, но работой не тревожь
B:> как думаешь, когда закончится пандемия коронавируса?
H:> Не знаю, но надеюсь, что завтра, мне она уже порядком поднадоела.
```
Общение стало затруднительным, кажется, что я упустил контекст.
```
B:> ты болела COVID-19?
H:> Да.


B:> У кого чего болит, тот с тем дома и сидит.
B:> как ты относишься к прививкам от гриппа?
H:> Положительно. Что не убивает - делает сильнее?
```
Пытался подыграть, но напоролся на очередную ошибку:
```
2021-11-26 16:16:42 ERROR   SimpleAnsweringMachine    Two consequent questions issued by bot test_bot: prev="ты простывала этой зимой?", new="тебе нравятся роли Сергея Безрукова?"
```
```
B:> не убивает тебя надежда
B:> блюда какой кухни тебе нравятся?
H:> Я люблю острую еду. Например, индийскую. Однако, я также люблю классические. Ребрышки или типа того.

B:> какие блюда любишь готовить из острого?
H:> Готовить? Я редко готовю что-то специфическое. Просто беру что есть и намешиваю в большую миску.


B:> и в большую миску наливаешь воду
H:> Нет, только если это суп, но порядок действий не такой. А что на счет тебя? Ты любишь перекусить?


B:> я люблю вкусно поесть
H:> Ты предпочитаешь фастфуд или готовить самостоятельно? 


B:> я люблю готовить самостоятельно
H:> Я часто включаю какой-нибудь фильм или подборку песен во время готовки, таким образом нахожу в себе силы и новые 
идеи для текущей задачи. А что на счет тебя? 
```
Ошибка!!
```
2021-11-26 16:24:20 ERROR   SimpleAnsweringMachine    Two consequent questions issued by bot test_bot: prev="ты могла бы прожить вообще без мяса?", new="какое из музыкальных направлений тебе по душе?"
```
```
B:> спроси меня о чем-нибудь
H:> Тебе нравится изобразительное искусство?
```
Неожиданный исход, кажется, что Вика не распознала вопрос в прошлой моей реплике и таким образом попросила его повторить.
```
B:> но в изобразительном искусстве ты предпочитаешь искусство
H:> Какой твой любимый фильм?
```
Тут уже я перестал понимать Вику, кажется, что она забыла озвучить промежуточную мысль. Я решил поменять тему.
```
B:> потому что он про роботов
B:> ты часто смотришь фильмы?
H:> Да, но я стараюсь сократить это время.

B:> ты любишь читать?
H:> Да, художественную литературу. Мне нравится бегбедер.

B:> ты бегаешь в нем?
```
Я прекратил общение, т.к. Вика начала пропускать фразы. Неправильную интерпретацию фамилии французского писателя я ей
простил.

 ### Выводы:

* ~~Я не умею общаться с девушками.~~
* Этому боту еще далеко до прохождения теста Тьюринга, иногда приходилось подстраивать свои реплики.
* Отсутствие фраз "Я не понимаю", "Что такое Бегбедер", "Объясни"... Учитывая, что база для обучения ограниченная и не 
содержит всех слов - бот должен пробовать обучаться в ходе разговора. Вместо обучения, бот пытается найти ближайшее 
похожее слово. (В обучении тоже есть свои проблемы)
* Попытка закидать человека вопросами кажется креативным решением, т.к. для выбора наиболее подходящего по контексту 
очередного вопроса, не обязательно полностью понимать смысл всех фраз, достаточно выделить ключевые слова и задать
новый вопрос.
* Попытка отвечать на вопросы слегка провалилась, бот обладает навыками текстового помощника, но на сложные вопросы,
требующие оценки или минимальный творческий подход, не способен.
* Кажется, что проект динамически развивается и бот обучается на современных диалогах, он минимально осведомлен о 
происходящем в мире (COVID-19).
* Интересно то, что при общении бот сразу ушел в крайности, спрашивал о группе "Ночные Снайперы" и об отношении к
Эминему, даже не поинтересовавшись любимым музыкальным жанром. Однако, попытка обсудить фильмы про Бэтмена, Клерков
и Бегбедера, как писателя, зашла в тупик.


#### [Запись](https://github.com/yaryabtsev/soc/blob/main/%D0%93%D0%BE%D0%BB%D0%BE%D1%81%20001.mp4?raw=true)

