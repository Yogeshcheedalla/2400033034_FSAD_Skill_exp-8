# 2400033034_FSAD_Skill_exp-8

## Skill 8: Spring Boot JPQL & Query Methods - FSAD Lab Experiment

### Overview
Demonstrates Spring Data JPA with custom query methods and JPQL queries for advanced database operations.

### Entity Class

```java
@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String department;
    private Double salary;
    
    // Constructors, getters, setters
}
```

### Repository Interface

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    // Derived Query Methods
    List<Employee> findByDepartment(String department);
    
    List<Employee> findByNameContaining(String name);
    
    List<Employee> findBySalaryGreaterThan(Double salary);
    
    List<Employee> findBySalaryLessThan(Double salary);
    
    Employee findByName(String name);
    
    List<Employee> findByDepartmentOrderBySalaryDesc(String department);
    
    // JPQL Queries
    @Query("SELECT e FROM Employee e WHERE e.department = :dept")
    List<Employee> findByDept(@Param("dept") String dept);
    
    @Query("SELECT e FROM Employee e WHERE e.salary > :salary")
    List<Employee> findHighSalaryEmployees(@Param("salary") Double salary);
    
    @Query("SELECT COUNT(e) FROM Employee e WHERE e.department = :dept")
    Long countByDepartment(@Param("dept") String department);
    
    @Query("SELECT AVG(e.salary) FROM Employee e WHERE e.department = :dept")
    Double getAverageSalary(@Param("dept") String department);
    
    @Query("SELECT e FROM Employee e WHERE e.name LIKE %:name%")
    List<Employee> searchByName(@Param("name") String name);
    
    @Query(value = "SELECT * FROM employees WHERE salary > ?1", nativeQuery = true)
    List<Employee> findHighSalaryEmployeesNative(Double salary);
}
```

### Service Class

```java
@Service
public class EmployeeService {
    @Autowired
    private EmployeeRepository employeeRepository;
    
    // Using derived query methods
    public List<Employee> getEmployeesByDepartment(String dept) {
        return employeeRepository.findByDepartment(dept);
    }
    
    public List<Employee> searchEmployees(String name) {
        return employeeRepository.findByNameContaining(name);
    }
    
    public List<Employee> getHighSalaryEmployees(Double salary) {
        return employeeRepository.findBySalaryGreaterThan(salary);
    }
    
    // Using JPQL queries
    public List<Employee> getByDepartmentJPQL(String dept) {
        return employeeRepository.findByDept(dept);
    }
    
    public Double getAvgSalary(String dept) {
        return employeeRepository.getAverageSalary(dept);
    }
    
    public Long getDepartmentCount(String dept) {
        return employeeRepository.countByDepartment(dept);
    }
}
```

### Query Method Naming Conventions

| Method Name | Query Generated |
|-------------|----------------|
| findByName | WHERE name = ? |
| findByNameAndDepartment | WHERE name = ? AND department = ? |
| findByNameOrDepartment | WHERE name = ? OR department = ? |
| findByNameStartingWith | WHERE name LIKE ? |
| findByNameEndingWith | WHERE name LIKE ? |
| findByNameContaining | WHERE name LIKE %?% |
| findBySalaryGreaterThan | WHERE salary > ? |
| findBySalaryLessThan | WHERE salary < ? |
| findBySalaryBetween | WHERE salary BETWEEN ? AND ? |
| findByDepartmentOrderBySalaryDesc | ORDER BY salary DESC |
| findByDepartmentOrderBySalaryAsc | ORDER BY salary ASC |

### JPQL Query Examples

```java
// Select all employees
@Query("SELECT e FROM Employee e")
List<Employee> getAllEmployees();

// Select with where clause
@Query("SELECT e FROM Employee e WHERE e.salary > :minSalary")
List<Employee> findEmployeesWithMinSalary(@Param("minSalary") Double minSalary);

// Aggregate functions
@Query("SELECT COUNT(e) FROM Employee e")
Long getTotalEmployees();

@Query("SELECT MAX(e.salary) FROM Employee e")
Double getMaxSalary();

@Query("SELECT AVG(e.salary) FROM Employee e")
Double getAverageSalary();

// Group by
@Query("SELECT e.department, COUNT(e) FROM Employee e GROUP BY e.department")
List<Object[]> getEmployeeCountByDepartment();
```

### Native SQL Queries

```java
@Query(value = "SELECT * FROM employees WHERE salary > ?1", nativeQuery = true)
List<Employee> findEmployeesWithNativeSQL(Double salary);

@Query(value = "SELECT DISTINCT department FROM employees", nativeQuery = true)
List<String> getAllDepartments();
```

### Assessment Criteria
- [ ] Repository extends JpaRepository
- [ ] Derived query methods implemented
- [ ] JPQL queries with @Query annotation
- [ ] Query parameters using @Param
- [ ] Native SQL queries tested
- [ ] Service layer using repository methods
- [ ] All queries tested and working

### Key Concepts
- **Derived Query Methods**: Spring automatically generates queries
- **JPQL**: Java Persistence Query Language
- **@Query**: Custom JPQL/Native SQL queries
- **@Param**: Named parameters in queries
- **nativeQuery**: SQL queries instead of JPQL
- **Projections**: Select specific columns

### References
- [Spring Data JPA Docs](https://spring.io/projects/spring-data-jpa)
- [Query Methods](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods)
- [JPQL Tutorial](https://www.baeldung.com/spring-data-jpa-query)

### Author
**Student ID:** 2400033034
**Experiment:** 8 - Spring Boot JPQL & Query Methods
**Last Updated:** March 2026
