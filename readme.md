# Prereform2modern
#### Преобразует текст из дореформенной орфографии в современную. Работает в Py3.  
---

### &emsp;&emsp;Запуск из командной строки:
```python
$ python3 prereform2modern/translit_from_string.py "Онъ"
```

```python
['prereform2modern/translit_from_string.py', 'Онъ']

Он{Онъ}
```

&emsp;Флаг __-t__ позволяет получить результат в формате __json__.
```python
$ python3 prereform2modern/translit_from_string.py -t "Онъ"
```

```python
['prereform2modern/translit_from_string.py', '-t', 'Онъ']
{"0": {"word": "Он", "old_word": "Онъ", "type": "word", "plain_word": null, "old_plain_word": null}}
```

### &emsp;&emsp;Запуск из интерпретатора:
```python
>>> from process import Processor
>>> text = 'офицiанскую' 
>>> t, change, w_edits, _json = Processor.process_text(text, show=False, delimiters=['', '{', '}'], 
check_brackets=False, print_log=False)
>>> print t
```
```
официанскую
```

### &emsp;&emsp;Параметры
```python
method Processor.process_text(text, show, delimiters, check_brackets, print_log=True)
```
* __text: unicode__

&emsp;Оригинальный текст в дореформенной орфографии.

* __show: boolean__

&emsp;Включает в результат заменённые слова в дореформенной орфографии. Если параметр `check_brackets=True`, то заменённые слова показываются при любом значении параметра show.

* __delimiters: list из трех элементов типа unicode__

&emsp;Используется для обозначения заменённых слов. Первый элемент помещается перед новым словом, а другие два элемента выделяют заменённое слово. Так, можно использовать скобки:
```python
delimiters=['', '{', '}']
text="примеръ"
```
```python
пример{примеръ}
```

&emsp;Или, например, теги XML (про использование тега \<choice> см. [здесь](https://en.wikipedia.org/wiki/Text_Encoding_Initiative#Choice_tag)):
```python
delimiters=['<choice><reg>', '</reg><orig>', '</orig></choice>']
```
```python
<choice><reg>пример</reg><orig>примеръ</orig></choice>
```

* __check_brackets: boolean__

&emsp;Помечает редакторскую правку.
```python
text='Пройдя комнату, такъ [называемую], офиціанскую'
delimiters=['', '{', '}']
check_brackets=True
```
```python
Пройдя комнату, так{такъ} <choice original_editorial_correction='[называемую]'><sic></sic>
<corr>называемую</corr></choice>, официанскую{офицiанскую}
```

* __print_log: boolean__

&emsp;Отображает замены, которые выполняет программа. Через пробел: символ старой орфографии, символ новой орфографии, слово после замены.
```python
print_log=True
```
```python
text='офицiанскую'
```
```python
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

```python
text='выраженіемъ'
```

```python
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

* __str_json: str__
```python
text='офицiанскую'
```
```python
'{"0": {"type": "word", "old_plain_word": null, "word": "официанскую", "old_word": "офицiанскую", 
"plain_word": null}}'
```
