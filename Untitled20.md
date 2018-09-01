

```python
%load_ext sql
```


```python
%sql oracle+cx_oracle://alan:42@192.168.0.105:1521/?service_name=orcl
```




    'Connected: alan@'




```python
%%sql

select * from employee
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>id</th>
        <th>dep_id</th>
        <th>chef_id</th>
        <th>name</th>
        <th>salary</th>
    </tr>
    <tr>
        <td>1</td>
        <td>1</td>
        <td>None</td>
        <td>ARTEM</td>
        <td>25000</td>
    </tr>
    <tr>
        <td>2</td>
        <td>1</td>
        <td>1</td>
        <td>IVAN</td>
        <td>27000</td>
    </tr>
    <tr>
        <td>3</td>
        <td>2</td>
        <td>1</td>
        <td>PETR</td>
        <td>23000</td>
    </tr>
    <tr>
        <td>4</td>
        <td>2</td>
        <td>3</td>
        <td>DMITRY</td>
        <td>22000</td>
    </tr>
    <tr>
        <td>5</td>
        <td>2</td>
        <td>4</td>
        <td>VASILY</td>
        <td>28000</td>
    </tr>
    <tr>
        <td>6</td>
        <td>2</td>
        <td>None</td>
        <td>AFANASIY</td>
        <td>33000</td>
    </tr>
    <tr>
        <td>7</td>
        <td>3</td>
        <td>None</td>
        <td>IGOR</td>
        <td>13000</td>
    </tr>
    <tr>
        <td>8</td>
        <td>3</td>
        <td>7</td>
        <td>KIRILL</td>
        <td>15000</td>
    </tr>
</table>



<hr>

### Задание №1
#### Вывести список сотрудников, получающих заработную плату большую, чем у непосредственного руководителя.


```python
%%sql
select EMP1.NAME from EMPLOYEE EMP1, EMPLOYEE EMP2 WHERE EMP1.CHEF_ID=EMP2.ID and EMP1.SALARY>EMP2.SALARY
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>name</th>
    </tr>
    <tr>
        <td>IVAN</td>
    </tr>
    <tr>
        <td>VASILY</td>
    </tr>
    <tr>
        <td>KIRILL</td>
    </tr>
</table>



<hr>

### Задание №2
#### Вывести список сотрудников, получающих максимальную заработную плату в своем отделе.


```python
%%sql

select employee.name,ms.max_salary
from employee, (select dep_id, max(salary) as max_salary from employee group by dep_id) ms
where employee.dep_id=ms.dep_id and employee.salary=ms.max_salary
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>name</th>
        <th>max_salary</th>
    </tr>
    <tr>
        <td>IVAN</td>
        <td>27000</td>
    </tr>
    <tr>
        <td>AFANASIY</td>
        <td>33000</td>
    </tr>
    <tr>
        <td>KIRILL</td>
        <td>15000</td>
    </tr>
</table>



<hr>

### Задание №3
#### Вывести список ID отделов, количество сотрудников которых не привышает 3 человек.


```python
%%sql
select dep_id from employee group by dep_id having count(*)<=3
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>dep_id</th>
    </tr>
    <tr>
        <td>1</td>
    </tr>
    <tr>
        <td>3</td>
    </tr>
</table>



<hr>

### Задание №4
#### Вывести список сотрудников, не имеющих назначенного руководителя, работающего в том же отделе.


```python
%%sql

select A.name
from employee A, employee B
where A.chef_id is null
or
(
A.chef_id=B.id
and
A.dep_id!=B.dep_id
)
group by A.name
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>name</th>
    </tr>
    <tr>
        <td>ARTEM</td>
    </tr>
    <tr>
        <td>AFANASIY</td>
    </tr>
    <tr>
        <td>PETR</td>
    </tr>
    <tr>
        <td>IGOR</td>
    </tr>
</table>



<hr>

### Задание №5
#### Найти список ID отделов с максимальной суммарной зарплатой сотрудников.


```python
%%sql

with max_salary (dep_id, sum_s) as(
    select dep_id,sum(salary) as sum_s from employee group by dep_id order by sum_s desc FETCH FIRST 1 ROWS ONLY),
    deps_summary (dep_id, summ) as(
    select dep_id, sum(salary) as summ from employee group by dep_id)
    select deps_summary.dep_id from deps_summary where deps_summary.summ=(select sum_s from max_salary)
```

     * oracle+cx_oracle://alan:***@192.168.0.105:1521/?service_name=orcl
    0 rows affected.





<table>
    <tr>
        <th>dep_id</th>
    </tr>
    <tr>
        <td>2</td>
    </tr>
</table>



 <hr>

<br>

<br> 

<br> 

<br> 

<br> 

<br> 

<br> 
