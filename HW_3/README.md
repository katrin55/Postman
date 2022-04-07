1) Необходимо залогиниться `http://162.55.220.72:5005/login` 
```
POST  
login : str   
password : str
```
- Приходящий токен необходимо передать во все остальные запросы:
```
var jsonData = pm.response.json();
pm.environment.set("token", jsonData.token);
```
2) `http://162.55.220.72:5005/user_info`
```
POST
age: int
salary: int
name: str
auth_token
```

#### Request (RAW JSON):    
```
{
"age": "{{age}}",  
"salary": "{{salary}}",  
"name": "{{name}}",  
"auth_token": "{{token}}"  
}
```

#### Responce:
```
{
"person": {
  "u_age": age,
  "u_name": [
    user_name,
    salary,
    age
    ],
  "u_salary_1_5_year": salary * 4
    },
  "qa_salary_after_12_months": salary * 2.9,
  "qa_salary_after_6_months": salary * 2,
  "start_qa_salary": salary
}
 ```

#### Тесты:
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Проверка структуры json в ответе.
```
var schema = 
{
"type": "object",
"properties": {
"person": {
  "type": "object",
	"properties": {
	"u_age": {
		"type": "integer"
    },
    "u_name": {	
        "type": "array",
        "items": [
        {"type": "string"},
        {"type": "number"},
        {"type": "number"}
                 ],
        },
    "u_salary_1_5_year": {					
    "type": "number",
    }
  }
},
    "qa_salary_after_12_months": {
    "type": "number",
      },
    "qa_salary_after_6_months": {			
    "type": "number",
      },
    "start_qa_salary": {
    "type": "number",
    }
  }
}

var jsonData = pm.response.json();
let reqData = JSON.parse(request.data);

pm.test ("JSON-schema valid", function() {
pm.expect(tv4.validate(jsonData, schema)).to.be.true;
});
```
- В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
```
pm.test("Check start_qa_salary = 1 * 50000" , function () {
pm.expect(jsonData.start_qa_salary).to.eql(+reqData.salary);
});

pm.test("Check qa_salary_after_6_months = 2 * 50000" , function () {
pm.expect(jsonData.start_qa_salary*2).to.eql(jsonData.qa_salary_after_6_months);
});

pm.test("Check qa_salary_after_12_months = 2.9 * 50000" , function () {
pm.expect(jsonData.start_qa_salary*2.9).to.eql(jsonData.qa_salary_after_12_months);
});

pm.test("u_salary_1_5_year = 4 * 50000" , function () {
pm.expect(jsonData.start_qa_salary*4).to.eql(jsonData.person.u_salary_1_5_year);
});
```

- Достать значение из поля `'u_salary_1.5_year'` и передать в поле salary запроса `http://162.55.220.72:5005/get_test_user`
```
pm.environment.set("super_salary", jsonData.person.u_salary_1_5_year);
```
3) `http://162.55.220.72:5005/new_data`
#### Request:
```
POST
age: int
salary: int
name: str
auth_token
```
#### Responce:
```
{'name': name,  
  'age': int (age),  
  'salary': [salary, str(salary*2), str(salary*3)]}
```
#### Тесты:
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Проверка структуры json в ответе.
```
var schema = 
{
"properties": {
"age":{
	"type": "integer"
},
"name":{ 
	"type": "string"
},
"salary":{
"type": "array",
"items": [
	{"type": "integer"},  
	{"type": "string"},  
	{"type": "string"} 
	 ],
	 }
     }
}

var jsonData = pm.response.json();
pm.test ("JSON-schema valid", function() {
pm.expect(tv4.validate(jsonData, schema)).to.be.true;
})
```
- В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
```
var reqData = request.data;
pm.test("Check salary = 1*50000" , function () {
pm.expect(reqData.salary*1).to.eql(jsonData.salary[0])
});

pm.test("Check salary = 2*50000" , function () {
pm.expect(reqData.salary*2).to.eql(+jsonData.salary[1]);
});

pm.test("Check salary = 3*50000" , function () {
pm.expect(reqData.salary*3).to.eql(+jsonData.salary[2]);
});
```
- проверить, что 2-й элемент массива salary больше 1-го и 0-го
```
pm.test("2 element > 1 and 0 elementa", function () {
    pm.expect((jsonData.salary[2] > jsonData.salary[1])).to.be.true
    pm.expect((jsonData.salary[2] > jsonData.salary[0])).to.be.true
});
```
4) `http://162.55.220.72:5005/test_pet_info`
#### Request:
```
POST
age: int
weight: int
name: str
auth_token
```
#### Responce:
```
{'name': name,  
 'age': age,  
 'daily_food':weight * 0.012,  
 'daily_sleep': weight * 2.5}
```
#### Тесты
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Проверка структуры json в ответе.
```
var schema = 
{
"properties": {
"age":{
  "type": "integer"
 },
"daily_food":{
  "type": "number"
 },
"daily_sleep":{
  "type": "number"
 },
"name":{
  "type": "string"
         }
    }
}

var jsonData = pm.response.json();
pm.test ("JSON-schema valid", function() {
pm.expect(tv4.validate(jsonData, schema)).to.be.true;
})
```
- В ответе указаны коэффициенты умножения weight, напишите тесты по проверке правильности результата перемножения на коэффициент.
```
var reqData = request.data;
pm.test("Check weight*0.012 =daily_food " , function () {
pm.expect(reqData.weight*0.012).to.eql(jsonData.daily_food)
});

pm.test("Check weight*2.5 =daily_sleep " , function () {
pm.expect(reqData.weight*2.5).to.eql(jsonData.daily_sleep)
});
```
5) `http://162.55.220.72:5005/get_test_user`
#### Request:
```
POST
age: int
salary: int
name: str
auth_token
```
#### Response: 
```
{'name': name,  
 'age':age,  
 'salary': salary,  
 'family': {'children':[['Alex', 24],['Kate', 12]],  
 'u_salary_1.5_year': salary * 4}
  }
```
#### Тесты:
- Статус код 200
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- Проверка структуры json в ответе.
```
var schema =
{
"properties": {
 "age":{
    "type": "string"
},
 "family": {
    "type": "object",
"properties": {
 "children": {
    "type": "array",
    "items": [
             	{ 
     "type": "array",
     "items": [
	      	{
          "type": "string"
      },
              	{
          "type": "integer"
      }
              ]
       },
              	{
          "type": "array",
          "items": [
              	{
          "type": "string"
       },
              	{
          "type": "integer"
       }
                   ]
       }
              ]
       },
          "u_salary_1_5_year":{
           "type": "integer"
       		}
	  }
   },
        "name":{
            "type": "string"
        },
        "salary":{
            "type": "integer"
        }
    }     
}

var jsonData = pm.response.json();
pm.test ("JSON-schema valid", function() {
pm.expect(tv4.validate(jsonData, schema)).to.be.true;
})
```
- Проверить что занчение поля name = значению переменной name из окружения 
```
let env_name = pm.environment.get('name')
pm.test("Check name = name-environment", function () {
pm.expect(jsonData.name).to.eql(env_name);
});
```
- Проверить что занчение поля age в ответе соответсвует отправленному в запросе значению поля age
```
var reqData = request.data;
pm.test("Check age", function () {
pm.expect(jsonData.age).to.eql(reqData.age);
});
```
