# Prereform2modern
#### Преобразует текст из дореформенной орфографии в современную.  
---
### &emsp;&emsp;Использование программы из командной строки в Py2:

```
$ python2.7 prereform2modern/translit_from_string.py "Онъ стоялъ подлѣ письменнаго стола"
```

```
['prereform2modern/translit_from_string.py', '\xd0\x9e\xd0\xbd\xd1\x8a \xd1\x81\xd1\x82\xd0\xbe\xd1\x8f\xd0\xbb\xd1\x8a \xd0\xbf\xd0\xbe\xd0\xb4\xd0\xbb\xd1\xa3 \xd0\xbf\xd0\xb8\xd1\x81\xd1\x8c\xd0\xbc\xd0\xb5\xd0\xbd\xd0\xbd\xd0\xb0\xd0\xb3\xd0\xbe \xd1\x81\xd1\x82\xd0\xbe\xd0\xbb\xd0\xb0']

./prereform2modern/word_tokenize.py:27: UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode - interpreting them as being unequal
  if litera in SYMBOLS['symbols']:
./prereform2modern/word_tokenize.py:41: UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode - interpreting them as being unequal
  if litera in SYMBOLS['numbers']:

Он{Онъ} стоял{стоялъ} подле{подлѣ} письменного{письменнаго} стола
```

&emsp;Флаг __-t__ позволяет получить результат в формате __json__
```
$ python2.7 prereform2modern/translit_from_string.py -t "Онъ"
```

```
{"0": {"type": "word", "old_plain_word": null, "word": "\u041e\u043d", "old_word": "\u041e\u043d\u044a", "plain_word": null}}
```

### &emsp;&emsp;Как это должно работать в Py3:
```
$ python3 prereform2modern/translit_from_string.py "Онъ"
```

```
['prereform2modern/translit_from_string.py', 'Онъ']
Он{Онъ}
```
&emsp;В Py3, кажется, сохраняется проблема с юникодом в объекте json:
```
$ python3 prereform2modern/translit_from_string.py -t "Онъ"
```
```
['prereform2modern/translit_from_string.py', '-t', 'Онъ']
{"0": {"word": "\u041e\u043d", "old_word": "\u041e\u043d\u044a", "type": "word", "plain_word": null, "old_plain_word": null}}
```
---
### &emsp;&emsp;Использование программы из интерпретатора


```
$ python2.7
>>> from process import Processor
>>> text = u'офицiанскую'  # например
>>> t, change, w_edits, _json = Processor.process_text(text, show=False, delimiters=[u'', u'{', u'}'], check_brackets=False, print_log=False)
>>> print t
```
```
официанскую
```

### &emsp;&emsp;Параметры
```
method Processor.process_text(text, show, delimiters, check_brackets, print_log=True)
```
* __text: unicode__

&emsp;Оригинальный текст в дореформенной орфографии.

* __show: boolean__

&emsp;Включает в результат заменённые слова в дореформенной орфографии. Если параметр check_brackets=True, то замененные слова в любом случае показываются, независимо от значения параметра show.

* __delimiters: list из трех элементов типа unicode__

&emsp;Используется для обозначения заменённых слов. Первый элемент помещается перед новым словом, а вторые два элемента выделяют заменённое слово. Так, можно использовать скобки:
```
delimiters=[u'', u'{', u'}']
text=u"примеръ"
```

```
пример{примеръ}
```
&emsp;Или, например, теги XML:
```
delimiters=[u'<choice><reg>', u'</reg><orig>', u'</orig></choice>']
```
```
<choice><reg>пример</reg><orig>примеръ</orig></choice>
```

* __check_brackets: boolean__

&emsp;Отображает редакторскую правку.
```
text=u'Пройдя комнату, такъ [называемую], офиціанскую', delimiters=[u'', u'{', u'}'], check_brackets=True
```
```
Пройдя комнату, так{такъ} <choice original_editorial_correction='[называемую]'><sic></sic><corr>называемую</corr></choice>, официанскую{офицiанскую}
```

* __print_log: boolean__

&emsp;Отображает замены, которые выполняет программа. Через пробел: символ старой орфографии, символ новой орфографии, слово после замены.
```
print_log=True
```
```
text = u'офицiанскую'
```
```
ѣ е офицiанскую
чьк чк офицiанскую
ъи ы офицiанскую
чьн чн офицiанскую
ияся иеся офицiанскую
i и официанскую
щию щью официанскую
ѳ ф официанскую
EL официанскую
````

```
text=u'выраженіемъ'
```

```
ѣ е выраженiем
чьк чк выраженiем
ъи ы выраженiем
чьн чн выраженiем
ияся иеся выраженiем
i и выражением
щию щью выражением
ѳ ф выражением
EL выражением
```

### &emsp;&emsp;Выдача
* __text: unicode__

&emsp;Преобразованный текст.

* __changes: unicode__

&emsp;Произведенные изменения в виде "офицiанскую --> официанскую".

* __wrong_edits: list__

&emsp;Пустой список :)

* __str_json: str__
```
text=u'офицiанскую'
```
```
'{"0": {"type": "word", "old_plain_word": null, "word": u"официанскую", "old_word": u"офицiанскую", "plain_word": null}}'
```

### &emsp;&emsp;Ключи old_plain_word и plain_word
&emsp;NULL...
