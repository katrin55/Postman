## HW_2 Postman

1) `http://162.55.220.72:5005/first`
- Отправить запрос.
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Проверить, что в body приходит правильный string
```
pm.test("Check String", function () {
    pm.expect(pm.response.text()).to.include("This is the first responce from server!");
});
```

2) `http://162.55.220.72:5005/user_info_3`
- Отправить запрос.
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Спарсить response body в json.   
`var jsonData = pm.response.json();`  
- Проверить, что name в ответе равно name's request (name вбить руками.)
```
pm.test("Check name", function () {
pm.expect(jsonData.name).to.eql("Kate");
});
```
- Проверить, что age в ответе равно age's request (age вбить руками.)
```
pm.test("Check age", function () {
pm.expect(jsonData.age).to.eql("25");
});
```
- Проверить, что salary в ответе равно salary's request (salary вбить руками.)
```
pm.test("Check salary", function () {
pm.expect(jsonData.salary).to.eql(50000);
});
```
- Спарсить request  
`let reqData = request.data;`  
`let salaryInt = (+reqData.salary)`
- Проверить, что name в ответе равно name's request (name забрать из request.)
```
pm.test("Check request name = response name" , function () {
pm.expect(jsonData.name).to.eql(reqData.name);
});
```
- Проверить, что age в ответе равно age's request (age забрать из request.)
```
pm.test("Check request age = response age" , function () {
pm.expect(jsonData.age).to.eql(reqData.age);
});

```
- Проверить, что salary в ответе равно salary's request (salary забрать из request.)
```
pm.test("Check request salary = response salary" , function () {
pm.expect(jsonData.salary).to.eql(salaryInt);
});
```
- Вывести в консоль параметр family из response.  
`console.log(jsonData.family)`
- Проверить что u_salary_1_5_year в ответе равно salary*4 (salary забрать из request)
```
pm.test("Check request u_salary_1_5_year = response u_salary_1_5_year" , function () {
pm.expect(jsonData.family.u_salary_1_5_year).to.eql(salaryInt*4);
});
```

3) `http://162.55.220.72:5005/object_info_3`
- Отправить запрос.
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

```
- Спарсить response body в json.  
`var jsonData = pm.response.json();`  
- Спарсить request  
`var reqData = pm.request.url.query.toObject();`  
- Проверить, что name в ответе равно name's request (name забрать из request.)
```
pm.test("Check request name = response name" , function () {
pm.expect(jsonData.name).to.eql(reqData.name);
});
```
- Проверить, что age в ответе равно age's request (age забрать из request.)
```
pm.test("Check request age = response age" , function () {
pm.expect(jsonData.age).to.eql(reqData.age);
});
```
- Проверить, что salary в ответе равно salary's request (salary забрать из request.)
```
pm.test("Check request salary = response salary" , function () {
pm.expect(jsonData.salary).to.eql(+reqData.name);
});

```
- Вывести в консоль параметр family из response.  
`console.log(jsonData.family);`  
- Проверить, что у параметра dog есть параметры name.
```
pm.test("Param dos has name" , function () {
pm.expect(jsonData.family.pets.dog).to.include.keys("name");
});
```
- Проверить, что у параметра dog есть параметры age.
```
pm.test("Param dos has age" , function () {
pm.expect(jsonData.family.pets.dog).to.include.keys("age");
});
```
- Проверить, что параметр name имеет значение Luky.
```
pm.test("Check dog's name is Luky" , function () {
pm.expect(jsonData.family.pets.dog.name).to.eql("Luky");
});
```
- Проверить, что параметр age имеет значение 4.
```
pm.test("Check dog's age is 4" , function () {
pm.expect(jsonData.family.pets.dog.age).to.eql(4);
});
```

4) `http://162.55.220.72:5005/object_info_4`
- Отправить запрос.
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Спарсить response body в json.  
`var jsonData = pm.response.json();`  
- Спарсить request.  
`var reqData = pm.request.url.query.toObject();`  
- Проверить, что name в ответе равно name's request (name забрать из request.)
```
pm.test("Check request name = response name" , function () {
pm.expect(jsonData.name).to.eql(reqData.name);
});
```
- Проверить, что age в ответе равно age's из request (age забрать из request.)
```
pm.test("Check request age = response age" , function () {
pm.expect(jsonData.age).to.eql(+reqData.age);
});
```
- Вывести в консоль параметр salary из request.  
`console.log(reqData.salary);`  
- Вывести в консоль параметр salary из response.  
`console.log(jsonData.salary);`  
- Вывести в консоль 0-й элемент параметра salary из response.  
`console.log(jsonData.salary[0]);`  
- Вывести в консоль 1-й элемент параметра salary параметр salary из response.  
`console.log(jsonData.salary[1]);`  
- Вывести в консоль 2-й элемент параметра salary параметр salary из response.  
`console.log(jsonData.salary[2]);`  
- Проверить, что 0-й элемент параметра salary равен salary из request (salary забрать из request.)
```
pm.test("Check 0 param salary = request salary" , function () {
pm.expect(jsonData.salary[0]).to.eql(+reqData.salary);
});
```
- Проверить, что 1-й элемент параметра salary равен salary*2 из request (salary забрать из request.)
```
pm.test("Check 0 param salary = request salary" , function () {
pm.expect(jsonData.salary[1]).to.eql(+reqData.salary*2);
});
```
- Проверить, что 2-й элемент параметра salary равен salary*3 из request (salary забрать из request.)
```
pm.test("Check 0 param salary = request salary" , function () {
pm.expect(jsonData.salary[2]).to.eql(+reqData.salary*3);
});
```
- Создать в окружении переменную salary
- Создать в окружении переменную age
- Создать в окружении переменную name
- Передать в окружение переменную name  
`pm.environment.set("name", jsonData.name);`  
- Передать в окружение переменную age  
`pm.environment.set("age", jsonData.age);`  
- Передать в окружение переменную salary  
`pm.environment.set("name", jsonData.name);`  
- Написать цикл который выведет в консоль по порядку элементы списка из параметра salary.
```
for (let i = 0; i < jsonData.salary.length; i++) {
	console.log( i +  " " + "элемент:"+ " " +jsonData.salary[i]);
}
```
5) `http://162.55.220.72:5005/user_info_2`
- Вставить параметр salary из окружения в request
- Вставить параметр age из окружения в age
- Вставить параметр name из окружения в name
- Отправить запрос.
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

```
- Спарсить response body в json.  
`var jsonData = pm.response.json();`
- Спарсить request.  
`var reqData = request.data;`
- Проверить, что json response имеет параметр start_qa_salary
```
pm.test("Json_responce has start_qa_salary " , function () {
pm.expect(jsonData).to.include.keys("start_qa_salary");
});
```
- Проверить, что json response имеет параметр qa_salary_after_6_months
```
pm.test("Json_responce has qa_salary_after_6_months " , function () {
pm.expect(jsonData).to.include.keys("qa_salary_after_6_months");
});
```
- Проверить, что json response имеет параметр qa_salary_after_12_months
```
pm.test("Json_responce has qa_salary_after_12_months " , function () {
pm.expect(jsonData).to.include.keys("qa_salary_after_12_months");
});
```
- Проверить, что json response имеет параметр qa_salary_after_1.5_year
```
pm.test("Json_responce has qa_salary_after_1.5_year " , function () {
pm.expect(jsonData).to.include.keys("qa_salary_after_1.5_year");
});
```
- Проверить, что json response имеет параметр qa_salary_after_3.5_years
```
pm.test("Json_responce has qa_salary_after_3.5_year " , function () {
pm.expect(jsonData).to.include.keys("qa_salary_after_3.5_years");
});
```
- Проверить, что json response имеет параметр person
``` 
pm.test("Json_responce has person " , function () {
pm.expect(jsonData).to.include.keys("person");
});
```
- Проверить, что параметр start_qa_salary равен salary из request (salary забрать из request.)
```
pm.test("Check start_qa_salary = request salary" , function () {
pm.expect(jsonData.start_qa_salary).to.eql(+reqData.salary);
});
```
- Проверить, что параметр qa_salary_after_6_months равен salary*2 из request (salary забрать из request.)
```
pm.test("Check qa_salary_after_6_months = request salary*2" , function () {
pm.expect(jsonData.qa_salary_after_6_months).to.eql(+reqData.salary*2);
});
```
- Проверить, что параметр qa_salary_after_12_months равен salary*2.7 из request (salary забрать из request.)
```
pm.test("Check qa_salary_after_12_months = request salary*2.7" , function () {
pm.expect(jsonData.qa_salary_after_12_months).to.eql(+reqData.salary*2.7);
});
```
- Проверить, что параметр qa_salary_after_1.5_year равен salary*3.3 из request (salary забрать из request.)
```
pm.test("Check qa_salary_after_1.5_year = request salary*3.3" , function () {
pm.expect(jsonData["qa_salary_after_1.5_year"]).to.eql(+reqData.salary*3.3);
});
```
- Проверить, что параметр qa_salary_after_3.5_years равен salary*3.8 из request (salary забрать из request.)
```
pm.test("Check qa_salary_after_3.5_year = request salary*3.8" , function () {
pm.expect(jsonData["qa_salary_after_3.5_years"]).to.eql(+reqData.salary*3.8);
});
```
- Проверить, что в параметре person, 1-й элемент из u_name равен salary из request (salary забрать из request.)
```
pm.test("Check in param person element u_name = request salary" , function () {
pm.expect(jsonData.person.u_name[1]).to.eql(+reqData.salary);
});
```
- Проверить, что что параметр u_age равен age из request (age забрать из request.)
```
pm.test("Check in param person element u_age = request age" , function () {
pm.expect(jsonData.person.u_age).to.eql(+reqData.age);
});
```
- Проверить, что параметр u_salary_5_years равен salary*4.2 из request (salary забрать из request.)
```
pm.test("Check in param person element u_salary_5_years = request salary*4.2" , function () {
pm.expect(jsonData.person.u_salary_5_years).to.eql(+reqData.salary*4.2);
});
```
- ***Написать цикл который выведет в консоль по порядку элементы списка из параметра person.
```
for(let i in jsonData.person) {
   if(typeof(jsonData.person[i]) === "object"){
       for(let k = 0; k < Object.keys(jsonData.person[i]).length; k++){
           console.log(jsonData.person[i][k]);
       }
   }
   else if(typeof(jsonData.person[i]) != "object") {
        console.log(jsonData.person[i]);
   }
}
```
