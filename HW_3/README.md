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

### Request (RAW JSON):    
```
{
"age": "{{age}}",  
"salary": "{{salary}}",  
"name": "{{name}}",  
"auth_token": "{{token}}"  
}
```

### Responce:
```
{
"person": {
  "u_age": 25,
  "u_name": [
    "Kate",
     50000,
     25
    ],
  "u_salary_1_5_year": 200000
    },
  "qa_salary_after_12_months": 145000.0,
  "qa_salary_after_6_months": 100000,
  "start_qa_salary": 50000
}
 ```

### Тесты:
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

4) Достать значение из поля `'u_salary_1.5_year'` и передать в поле salary запроса `http://162.55.220.72:5005/get_test_user`
```
pm.environment.set("super_salary", jsonData.person.u_salary_1_5_year);
```
