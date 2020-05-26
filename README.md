# xls_export_php
Экспорт таблицы в Excel при помощи PHP

Допустим у нас имеется любая HTML таблица (table).
Необходимо выгрузить её в Excel при нажатии кнопки.

Вот решение:

Добавим кнопку "сформировать Excel":
```html
<form action="xls.php" method="POST">
      <input type="hidden" name="data" id="xls_data" value="">
      <input type="hidden" name="report_name" value="x_report">
      <input type="submit" value="Excel" class="r_report_button--excel" disabled>
  </form>
```

<b>xls_data</b> - Данные, которые передаем, в данном случае таблица.<br>
<b>report_name</b> - Имя отчета<br>
<b>r_report_button</b> - Сабмит. Кнопка отправки.<br>

Теперь скрипт (jQuery), который поможет отправить таблицу POST-запросом:

```js

  $(document).ready(function() {
      $('.r_report_button--excel').attr('disabled', false);
      var excel_data = $('#x_report_table').html(); 
      var report_data = $('.report_table').html(); 
      $('#xls_data').val(excel_data);
      $('#report_data').val(report_data);
  });
```

Активируем кнопку, как страница прогрузится до конца (нужно, если реально огромный объем данных).<br>
Hidden поля заполняются данными из таблицы. При нажатии кнопки запрос отправляется в файл xls.php

```php
header('Content-Type: application/vnd.ms-excel; charset=utf-8;');  
header('Content-disposition: attachment; filename='.$_POST["report_name"].'_'.date("d-m-Y").'.xls');  
header("Content-Transfer-Encoding: binary ");
echo '  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
   <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
 <head>
 <meta http-equiv="content-type" content="text/html; charset=utf-8" />
 <meta name="author" content="Sadovikow" />
 <title>Demo</title>
 </head>
 <body>';
echo '<table border="1" cellpadding="15">';
echo $_POST["data"];  
echo '</table>';
echo '<br>';
echo '</body></html>';
```

В данном коде реализована формирование таблицы при помощи HTML. Заголовки нужны для корректной кодировки, очень важно, если имеется кириллица.

<b>Всё очень просто, абсолютно любая таблица может спокойно быть экспортирована в Excel!</b>

Более подробно <a href="https://www.redsgroup.ru/workshop/rework/table_in_excel/" title="Экспорт таблицы HTML в Excel">Здесь</a>
