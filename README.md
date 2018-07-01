# sqlTasks

#### 1. Вывести список сотрудников, получающих заработную плату большую, чем у непосредственного руководителя
```sql
select emp1.name from employee as emp1, employee as emp2 where emp1.cheif_id=emp2.id and emp1.salary>emp2.salary
```

#### 2. Вывести список сотрудников, получающих максимальную заработную плату в своем отделе
```sql
select name, salary, departament_id from employee group by departament_id order by salary desc 
```

#### 3. Вывести список ID отделов, количество сотрудников которых не привышает 3 человек
```sql
select departament_id from employee group by departament_id having count(*)<=3
```

#### 4. Вывести список сотрудников, не имеющих назначенного руководителя, работающего в том же отделе
```sql
select emp1.name from employee as emp1 where emp1.cheif_id='' or emp1.departament_id!=(select departament_id from employee where id=emp1.cheif_id)
```

#### 5. Найти список ID отделов с максимальной суммарной зарплатой сотрудников
```sql
select departament_id, sum(salary) as sum_salary from employee group by departament_id order by sum_salary desc limit 1
```

####
```sql

```