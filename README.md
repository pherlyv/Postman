# Postman

## Переменные

### Установка переменных

* Глобальные переменные
```js
pm.globals.set('variable name', "value");
```

* Локальная переменная 
```js
var variable_name = value;
```

* Переменная среды
```js
pm.environment.set('variable_name' , 'value');
```

## Получение переменных
* Глобальные переменные
```js
pm.globals.get('variable_name');
```

*Переменная среды
```js
pm.environment.get('varibable_name');
```

* Переменная данных
```js
pm.iterationData.get('variable_name');
```

* Локальная переменная
```js
variable_name;
```

## Очистка переменных
*Глобальные переменные (только одна)
```js
pm.globals.unset('variable_name');
```

* Глобальные переменные (все)
```js
pm.globals.clear();
```

* Переменная среды (только одна)
```js
pm.environment.unset('variable_name');
```

* Переменная среды (все)
```js
pm.environment.clear();
```

## Ассерты
### Ответ
Различные коды относящиеся к ответам запроса
* Ответ содержит строку
```js
pm.test("String found", function(){
pm.expect(pm.response.text()).to.include("string you want to search");
});
```

* Тело ответа равно строке
```jspm.test("Body is equal to string", function(){
pm.response.to.have.body("string you want to check");
});
```
### Статус код
Код, относящийся к статус кодам в Postman
* Один статус код
```js
pm.response.to.have.status(status_code);
```

* Несколько статус кодов
```js
pm.expect(pm.response.code).to.be.oneOf([status_code, status_code]);
```

### Время ответа
```js
pm.expect(pm.response.responseTime.to.be.below(time));
```

### Проверка значения JSON
```js
pm.test("Your_Test_Name", function(){
var jsonData = pm.response.json();
pm.expect(jsonData.value).to.eql(value);
});
```

### Заголовок содержит Content-Type
```js
pm.test("Your_Test_Name", function(){
pm.response.to.have.header("Content-Type");
});
```

### Ответ содержит куки sessionID
```js
pm.expect(pm.cookies.has('sessionID')).to.be.true;
```

### Тело ответа
Код относящийся к телу ответа
* Точное совпадение
```js
pm.response.to.have.body("OK");
pm.response.to.have.body('{"success"=true}');
```

* Частичное совпадение
```js
pm.expect(pm.response.text()).to.include('ToolsQA');
```

### Отправить асинхронный запрос
```js
pm.sendRequest("https://postman-echo.com/get", function(err, response){
console.log(response.json());
});
```

## JSON ответ
* Распарсить тело
```js
var jsonData = pm.response.json();
```

* Проверить количество массивов в ответе
```js
pm.test("ISBN Count", function () {
pm.expect(2).to.eql(pm.response.json().arrayName.length);
});
```

* Проверить конкретное значение внутри массива
В этом примере проверяется конкретный номер ISBN среди всех книг, полученных в ответе, и возвращается значение true, если оно найдено
```js
pm.test("Test Name", function () {
var result;
for (var loop = 0; loop < pm.response.json().arrayName.length; loop++)
{
if (pm.response.json().arrayName[loop].arrayElement=== pm.variables.get("arrayElementValue")){
result=true;
break;
}
}
pm.expect(true).to.eql(result);
});
```

В двух приведенных выше примерах использовался Javascript, поскольку Postman Sandbox работает с javascript. Этот код не имеет конкретного отношения к Postman. 

* Проверить значение
```js
pm.expect(jsonData.age).to.eql(value);
```
```js
pm.expect(jsonData.name).to.eql("string");
```

* Преобразование тела XML в объект JSON
```js
var jsonObject = xml2Json(responseBody);
```

## Рабочие процессы
* Установка следующего запроса
```js
postman.setNextRequest("Request Name");
```
* Прекратить выполнение запроса
```js
postman.setNextRequest(null);
```

## Библиотека ассертов Chai
* Найти число в массиве
```js
pm.test(“Number included”, function(){
pm.expect([1,2,3]).to.include(3);
});
```

* Проверить, пустой ли массив
```js
pm.test(“Empty Array”, function(){
pm.expect([2]).to.be.an(‘array’).that.is.empty;
});
```

## Спарсить Request
* POST from data
```js
let post_form_data = request.data;
```
* POST RAW
```js
let post_raw = JSON.parse(request.data);
```
* GET
```js
let get_params = pm.request.url.query.toObject();
```
