# Конвертор текстовых форматов Pandoc
Версия документа: 0.04

## Небольшое введение

Что такое [Pandoc](http://pandoc.org)? Эта программа, которой нам так не хватало в давние времена. Она умеет конвертировать документы множества форматов друг в друга, облегчая разработку и публикацию статей, справок, веб-документов, книг и многое другое. Написана эта чудесная программа на функциональном языке программирования __Haskell__ и распростаняется под лицензией [GPL](http://www.gnu.org/copyleft/gpl.html), то есть бесплатно. Ее автор - [John MacFarlane](http://johnmacfarlane.net).

Наиболее удобно использовать Pandoc для преобразования исходного текста, написанного с использованием разметки __Markdown__ в различные "публичные" форматы типа _html_, _tex_ или даже _docx_ и _pdf_. Дело в том, что разметка _Markdown_ предоставляет очень простой язык разметки документов, который можно изучить за несколько минут и в дальнейшем использовать повсеместно. 

## Редакторы с поддержкой разметки _Markdown_ 

### в _Mac OS X_

В Mac OS X существуют многочисленные редакторы с поддержкой _Markdown_. И с каждым годом их становится все больше и больше. Основное преимущество этого вида разметки заключается в простоте и удобстве, когда речь идет о создании документов с несложной структурой и текстовым содержанием (нет сложных формул, таблиц).

Перечислим некоторые популярные редакторы, специально разработанные для работы с _Markdown_:

- **Mou** 
- **MacDown**
- **Haroopad**
- **Byword**
- **LightPaper**
- **Atom**
- **Texts** 
- **iA Writer** 


## Элементы языка разметки _Markdown_

### Включение блоков с кодом

Простейший способ вставить блок кода - сделать отступ на 4 пробела:

    #include <stdio.h>
    int main()
    {
       printf("Hello, world!\n");
       return 0;
    }
Другой способ: поместить три символа обратной коавычки перед блоком и три символа после:

```
   ```
   #include <stdio.h>
   int main()
   {
     printf("Hello, world!\n");
     return 0;
   }
   ```
```
Если указать язык программирования, то появится синтаксическое раскрашивание:

```
   ```с++
   #include <stdio.h>
   int main()
   {
     printf("Hello, world!\n");
     return 0;
   }
   ```
```

Выглядит это так:

```c++
#include <stdio.h>
int main()
{
   printf("Hello, world!\n");
   return 0;
}
```

## Преобразование документов _Markdown_ в различные форматы

### Преобразование в DOCX

Самый простой способ сделать это:

    pandoc -o file.docx -f markdown -t docx file.md
    
По-умолчанию, используется стандартый шаблон Word-файла. При желании, можно взять сгенерированный файл, поправить его и использовать в качестве шаблона:

    pandoc -o file.docx -f markdown -t docx file.md --reference-docx=template.docx
    
### Преобразование в PDF

Преобразование происходит через TeX-формат с последующей компиляцией в PDF. Опция _--latex-engine_ задает программу для компиляции.

Варианты:

- pdflatex
- xelatex
- luatex

Выбор xelatex позволяет использовать True Type шрифты

    pandoc -o file.pdf -f markdown  --latex-engine=xelatex  file.md 

Для поддержки русского языка можно использовать опцию  _lang=russian_:

    pandoc -o file.pdf -f markdown  --latex-engine=pdflatex -V lang=russian file.md    

Опция -V позволяет устанавливать многие параметры TeX-файла, например указать шрифт и базовый размер:

    pandoc -o file.pdf -f markdown  --latex-engine=xelatex -V mainfont="Ubuntu" -V fontsize=12pt file.md
    
А так можно еще и задать поля документа:

    pandoc -o file.pdf -f markdown  --latex-engine=xelatex -V mainfont="Ubuntu" -V fontsize=12pt file.md -V geometry:margin=2cm    

Но наиболее гибкий способ заключается в использовании шаблонов. С помощью команды

    pandoc -D latex > t.tex 
    
мы копируем стандартные настройки TeX-шаблона в файл _t.tex_, который можно поправить руками, а затем указать при конвертации в PDF:

    pandoc -o test2.pdf -f markdown  --latex-engine=xelatex -V mainfont="Arial" -V fontsize=12pt test2.md -V geometry:margin=2cm --template=t.tex
    
В частности, в шаблоне можно изменить масштабирование шрифтов, формат документа и многое другое.

Другой пример с использованием параметров управления форматом бумаги:

    pandoc -V geometry:paperwidth=4in -V geometry:paperheight=6in -V geometry:margin=.5in -o file.pdf file.md
    
Здесь задается ширина листа, высота и значение полей.

### Управление шрифтами

В качестве значения опции _-V_ можно указать несколько значений для шрифтов:

- mainfont
- sansfont
- monofont

Каждый из этих типов шрифтов используется в документе для различных целей (например, для оформления листингов программ или выделения различных фрагментов текста).