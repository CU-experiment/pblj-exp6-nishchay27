Question1. Write a program to sort a list of Employee objects (name, age, salary) using lambda expressions.
CODE: import java.util.*;
class Employee {
    String name;
    int age;
    double salary;

    public Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
    public String toString() {
        return name + " (" + age + ", $" + salary + ")";
    }
}

public class EmployeeSorter {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Dhanshiv Kumar Singh", 21, 50000),
            new Employee("Aditya Raj", 21, 70000),
            new Employee("Hritik Ranjan Rai", 35, 60000),
            new Employee("Himanshu", 28, 65000)
        );

        employees.sort(Comparator.comparing(emp -> emp.name));
        System.out.println("Sorted by name: " + employees);

        employees.sort(Comparator.comparingInt(emp -> emp.age));
        System.out.println("Sorted by age: " + employees);

        employees.sort(Comparator.comparingDouble(emp -> emp.salary));
        System.out.println("Sorted by salary: " + employees);
    }
}
Question2. Create a program to use lambda expressions and stream operations to filter students scoring above 75%, sort them by marks, and display their names.
CODE: import java.util.*;
import java.util.stream.*;

class Student {
    String name;
    double marks;

    public Student(String name, double marks) {
        this.name = name;
        this.marks = marks;
    }

    public String getName() {
        return name;
    }

    public double getMarks() {
        return marks;
    }
}

public class StudentFilter {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Dhanshiv Kumar Singh", 82),
            new Student("Aditya Raj", 67),
            new Student("Hritik Ranjan", 91),
            new Student("Himanshu", 74),
            new Student("Chirag", 88)
        );

        List<String> topStudents = students.stream()
            .filter(s -> s.getMarks() > 75) // Filter students scoring above 75%
            .sorted(Comparator.comparingDouble(Student::getMarks).reversed()) // Sort by marks descending
            .map(Student::getName) // Extract names
            .collect(Collectors.toList());

        System.out.println("Students scoring above 75% (sorted by marks):");
        topStudents.forEach(System.out::println);
    }
}
Question3. Write a Java program to process a large dataset of products using streams. Perform operations such as grouping products by category, finding the most expensive product in each category, and calculating the average price of all products.
CODE: import java.util.*;
import java.util.stream.Collectors;

class Product {
    private String name;
    private String category;
    private double price;

    public Product(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public String getCategory() {
        return category;
    }

    public double getPrice() {
        return price;
    }
    public String toString() {
        return String.format("%s (₹%.2f)", name, price);
    }
}

public class ProductProcessor {
    public static void main(String[] args) {
        List<Product> products = Arrays.asList(
            new Product("Laptop", "Electronics", 75000),
            new Product("Smartphone", "Electronics", 45000),
            new Product("Headphones", "Electronics", 5000),
            new Product("Tablet", "Electronics", 30000),
            new Product("Sofa", "Furniture", 55000),
            new Product("Chair", "Furniture", 12000),
            new Product("Table", "Furniture", 20000),
            new Product("T-shirt", "Clothing", 1000),
            new Product("Jeans", "Clothing", 2500),
            new Product("Jacket", "Clothing", 5000)
        );

        Map<String, List<Product>> productsByCategory = products.stream()
                .collect(Collectors.groupingBy(Product::getCategory));

        System.out.println("Products grouped by category:");
        productsByCategory.forEach((category, productList) -> {
            System.out.println(category + ": " + productList);
        });

        Map<String, Optional<Product>> mostExpensiveByCategory = products.stream()
                .collect(Collectors.groupingBy(
                        Product::getCategory,
                        Collectors.maxBy(Comparator.comparingDouble(Product::getPrice))
                ));

        System.out.println("\nMost expensive product in each category:");
        mostExpensiveByCategory.forEach((category, product) -> 
            System.out.println(category + ": " + product.orElse(null))
        );

        double averagePrice = products.stream()
                .mapToDouble(Product::getPrice)
                .average()
                .orElse(0.0);

        System.out.println("\nAverage price of all products: ₹" + String.format("%.2f", averagePrice));
    }
}
