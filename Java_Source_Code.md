# Student Grade Management System - Java Source Code

## 1. Student.java (Base Class)

```java
import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;

public class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private int studentID;
    private String name;
    private String email;
    private String password;
    private String role;
    private List<Grade> grades;
    private String enrollmentDate;
    
    // Constructor
    public Student(int studentID, String name, String email, String password, String role) {
        this.studentID = studentID;
        this.name = name;
        this.email = email;
        this.password = password;
        this.role = role;
        this.grades = new ArrayList<>();
        this.enrollmentDate = new java.time.LocalDate.now().toString();
    }
    
    // Getters and Setters
    public int getStudentID() {
        return studentID;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
    
    public String getPassword() {
        return password;
    }
    
    public void setPassword(String password) {
        this.password = password;
    }
    
    public String getRole() {
        return role;
    }
    
    public List<Grade> getGrades() {
        return grades;
    }
    
    // Grade Management Methods
    public void addGrade(Grade grade) {
        if (grade != null && grade.isValid()) {
            // Check for duplicate subject
            for (Grade g : grades) {
                if (g.getSubject().equalsIgnoreCase(grade.getSubject())) {
                    System.out.println("Grade for " + grade.getSubject() + " already exists!");
                    return;
                }
            }
            grades.add(grade);
            System.out.println("Grade added successfully!");
        } else {
            System.out.println("Invalid grade!");
        }
    }
    
    public void removeGrade(String subject) {
        grades.removeIf(g -> g.getSubject().equalsIgnoreCase(subject));
        System.out.println("Grade removed successfully!");
    }
    
    public Grade getGradeBySubject(String subject) {
        for (Grade g : grades) {
            if (g.getSubject().equalsIgnoreCase(subject)) {
                return g;
            }
        }
        return null;
    }
    
    public void updateGrade(String subject, double newValue) {
        Grade grade = getGradeBySubject(subject);
        if (grade != null) {
            grade.setGradeValue(newValue);
            System.out.println("Grade updated successfully!");
        } else {
            System.out.println("Subject not found!");
        }
    }
    
    // Calculation Methods
    public double calculateAverage() {
        if (grades.isEmpty()) return 0;
        double sum = 0;
        for (Grade g : grades) {
            sum += g.getGradeValue();
        }
        return sum / grades.size();
    }
    
    public double calculateMedian() {
        if (grades.isEmpty()) return 0;
        
        List<Double> values = new ArrayList<>();
        for (Grade g : grades) {
            values.add(g.getGradeValue());
        }
        values.sort(null);
        
        int size = values.size();
        if (size % 2 == 0) {
            return (values.get(size / 2 - 1) + values.get(size / 2)) / 2;
        } else {
            return values.get(size / 2);
        }
    }
    
    public double calculateGPA() {
        if (grades.isEmpty()) return 0;
        
        double totalPoints = 0;
        for (Grade g : grades) {
            double gradeValue = g.getGradeValue();
            if (gradeValue >= 90) totalPoints += 4.0;
            else if (gradeValue >= 80) totalPoints += 3.8;
            else if (gradeValue >= 70) totalPoints += 3.5;
            else if (gradeValue >= 60) totalPoints += 3.0;
            else if (gradeValue >= 50) totalPoints += 2.0;
            else if (gradeValue >= 40) totalPoints += 1.0;
            else totalPoints += 0;
        }
        return totalPoints / grades.size();
    }
    
    public String getPerformanceCategory() {
        double gpa = calculateGPA();
        if (gpa >= 3.5) return "EXCELLENT";
        else if (gpa >= 3.0) return "GOOD";
        else if (gpa >= 2.5) return "AVERAGE";
        else if (gpa >= 2.0) return "SATISFACTORY";
        else return "POOR";
    }
    
    public double getHighestGrade() {
        if (grades.isEmpty()) return 0;
        double max = 0;
        for (Grade g : grades) {
            if (g.getGradeValue() > max) {
                max = g.getGradeValue();
            }
        }
        return max;
    }
    
    public double getLowestGrade() {
        if (grades.isEmpty()) return 0;
        double min = 100;
        for (Grade g : grades) {
            if (g.getGradeValue() < min) {
                min = g.getGradeValue();
            }
        }
        return min;
    }
    
    @Override
    public String toString() {
        return "Student{" +
                "ID=" + studentID +
                ", Name='" + name + '\'' +
                ", Email='" + email + '\'' +
                ", Role='" + role + '\'' +
                ", Grades=" + grades.size() +
                ", Enrollment='" + enrollmentDate + '\'' +
                '}';
    }
}
```

## 2. Grade.java

```java
import java.io.Serializable;

public class Grade implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private int gradeID;
    private String subject;
    private double gradeValue;
    private char letterGrade;
    private String recordDate;
    private static int gradeCounter = 1000;
    
    public Grade(String subject, double gradeValue) {
        this.gradeID = ++gradeCounter;
        this.subject = subject;
        this.gradeValue = gradeValue;
        this.letterGrade = convertToLetterGrade();
        this.recordDate = new java.time.LocalDate.now().toString();
    }
    
    public int getGradeID() {
        return gradeID;
    }
    
    public String getSubject() {
        return subject;
    }
    
    public void setSubject(String subject) {
        this.subject = subject;
    }
    
    public double getGradeValue() {
        return gradeValue;
    }
    
    public void setGradeValue(double gradeValue) {
        if (gradeValue >= 0 && gradeValue <= 100) {
            this.gradeValue = gradeValue;
            this.letterGrade = convertToLetterGrade();
        }
    }
    
    public char getLetterGrade() {
        return letterGrade;
    }
    
    public String getRecordDate() {
        return recordDate;
    }
    
    public char convertToLetterGrade() {
        if (gradeValue >= 90) return 'A';
        else if (gradeValue >= 80) return 'B';
        else if (gradeValue >= 70) return 'C';
        else if (gradeValue >= 60) return 'D';
        else return 'F';
    }
    
    public boolean isValid() {
        return gradeValue >= 0 && gradeValue <= 100 && subject != null && !subject.isEmpty();
    }
    
    @Override
    public String toString() {
        return String.format("%s: %.1f (%c)", subject, gradeValue, letterGrade);
    }
}
```

## 3. AdminUser.java

```java
public class AdminUser extends Student {
    private static final long serialVersionUID = 1L;
    
    public AdminUser(int studentID, String name, String email, String password) {
        super(studentID, name, email, password, "ADMIN");
    }
    
    public void manageStudentGrades(Student student, String operation, String subject, double value) {
        switch (operation.toLowerCase()) {
            case "add":
                Grade grade = new Grade(subject, value);
                student.addGrade(grade);
                break;
            case "update":
                student.updateGrade(subject, value);
                break;
            case "delete":
                student.removeGrade(subject);
                break;
            default:
                System.out.println("Invalid operation!");
        }
    }
    
    public void viewAllStudentDetails(Student student) {
        System.out.println("\n========== STUDENT DETAILS ==========");
        System.out.println("Name: " + student.getName());
        System.out.println("ID: " + student.getStudentID());
        System.out.println("Email: " + student.getEmail());
        System.out.println("Total Grades: " + student.getGrades().size());
        System.out.println("GPA: " + String.format("%.2f", student.calculateGPA()));
        System.out.println("Performance: " + student.getPerformanceCategory());
        System.out.println("=====================================\n");
    }
}
```

## 4. RegularUser.java

```java
public class RegularUser extends Student {
    private static final long serialVersionUID = 1L;
    
    public RegularUser(int studentID, String name, String email, String password) {
        super(studentID, name, email, password, "STUDENT");
    }
    
    public void viewOwnGrades() {
        System.out.println("\n========== YOUR GRADES ==========");
        for (Grade g : this.getGrades()) {
            System.out.println(g);
        }
        System.out.println("================================\n");
    }
    
    public void viewOwnPerformance() {
        System.out.println("\n========== YOUR PERFORMANCE ==========");
        System.out.println("Average: " + String.format("%.2f", this.calculateAverage()));
        System.out.println("GPA: " + String.format("%.2f", this.calculateGPA()));
        System.out.println("Performance Category: " + this.getPerformanceCategory());
        System.out.println("=======================================\n");
    }
}
```

## 5. GradeService.java

```java
import java.util.ArrayList;
import java.util.List;

public class GradeService {
    
    public boolean addGrade(Student student, Grade grade) {
        try {
            if (grade.isValid()) {
                student.addGrade(grade);
                return true;
            }
            return false;
        } catch (Exception e) {
            System.out.println("Error adding grade: " + e.getMessage());
            return false;
        }
    }
    
    public boolean updateGrade(Student student, String subject, double value) {
        try {
            if (value >= 0 && value <= 100) {
                student.updateGrade(subject, value);
                return true;
            }
            return false;
        } catch (Exception e) {
            System.out.println("Error updating grade: " + e.getMessage());
            return false;
        }
    }
    
    public boolean deleteGrade(Student student, String subject) {
        try {
            student.removeGrade(subject);
            return true;
        } catch (Exception e) {
            System.out.println("Error deleting grade: " + e.getMessage());
            return false;
        }
    }
    
    public Student findTopPerformer(List<Student> students) {
        if (students.isEmpty()) return null;
        
        Student topPerformer = students.get(0);
        for (Student s : students) {
            if (s.calculateGPA() > topPerformer.calculateGPA()) {
                topPerformer = s;
            }
        }
        return topPerformer;
    }
    
    public List<Student> findAtRiskStudents(List<Student> students) {
        List<Student> atRiskStudents = new ArrayList<>();
        for (Student s : students) {
            if (s.calculateGPA() < 2.0) {
                atRiskStudents.add(s);
            }
        }
        return atRiskStudents;
    }
    
    public double calculateClassAverage(List<Student> students) {
        if (students.isEmpty()) return 0;
        
        double sum = 0;
        for (Student s : students) {
            sum += s.calculateAverage();
        }
        return sum / students.size();
    }
}
```

## 6. StudentService.java

```java
import java.util.ArrayList;
import java.util.List;

public class StudentService {
    private List<Student> students;
    
    public StudentService() {
        this.students = new ArrayList<>();
    }
    
    public boolean registerStudent(int id, String name, String email, String password) {
        try {
            Student student = new RegularUser(id, name, email, password);
            students.add(student);
            System.out.println("Student registered successfully!");
            return true;
        } catch (Exception e) {
            System.out.println("Error registering student: " + e.getMessage());
            return false;
        }
    }
    
    public boolean registerAdmin(int id, String name, String email, String password) {
        try {
            AdminUser admin = new AdminUser(id, name, email, password);
            students.add(admin);
            System.out.println("Admin registered successfully!");
            return true;
        } catch (Exception e) {
            System.out.println("Error registering admin: " + e.getMessage());
            return false;
        }
    }
    
    public Student getStudentById(int id) {
        for (Student s : students) {
            if (s.getStudentID() == id) {
                return s;
            }
        }
        return null;
    }
    
    public Student getStudentByEmail(String email) {
        for (Student s : students) {
            if (s.getEmail().equalsIgnoreCase(email)) {
                return s;
            }
        }
        return null;
    }
    
    public List<Student> getAllStudents() {
        return new ArrayList<>(students);
    }
    
    public boolean deleteStudent(int id) {
        Student student = getStudentById(id);
        if (student != null) {
            students.remove(student);
            System.out.println("Student deleted successfully!");
            return true;
        }
        return false;
    }
}
```

## 7. AuthenticationService.java

```java
public class AuthenticationService {
    
    private static final String ADMIN_PASSWORD_HASH = "admin@123";
    
    public Student authenticate(StudentService studentService, String email, String password) {
        try {
            Student student = studentService.getStudentByEmail(email);
            if (student != null && student.getPassword().equals(password)) {
                System.out.println("Authentication successful!");
                return student;
            }
            System.out.println("Invalid email or password!");
            return null;
        } catch (Exception e) {
            System.out.println("Authentication error: " + e.getMessage());
            return null;
        }
    }
    
    public boolean validatePassword(String password) {
        return password != null && password.length() >= 6;
    }
    
    public boolean validateEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }
}
```

## 8. ReportService.java

```java
import java.io.FileWriter;
import java.io.IOException;
import java.util.List;

public class ReportService {
    
    public String generateStudentReport(Student student) {
        StringBuilder report = new StringBuilder();
        report.append("\n========== STUDENT PERFORMANCE REPORT ==========\n");
        report.append("Name: ").append(student.getName()).append("\n");
        report.append("ID: ").append(student.getStudentID()).append("\n");
        report.append("Email: ").append(student.getEmail()).append("\n");
        report.append("\n--- Subject Grades ---\n");
        
        for (Grade g : student.getGrades()) {
            report.append(String.format("%s: %.1f (%c)\n", g.getSubject(), g.getGradeValue(), g.getLetterGrade()));
        }
        
        report.append("\n--- Statistics ---\n");
        report.append(String.format("Average: %.2f\n", student.calculateAverage()));
        report.append(String.format("Median: %.2f\n", student.calculateMedian()));
        report.append(String.format("Highest: %.1f\n", student.getHighestGrade()));
        report.append(String.format("Lowest: %.1f\n", student.getLowestGrade()));
        report.append(String.format("GPA: %.2f\n", student.calculateGPA()));
        report.append("\n--- Performance Category ---\n");
        report.append(student.getPerformanceCategory()).append("\n");
        report.append("================================================\n\n");
        
        return report.toString();
    }
    
    public String generateClassReport(List<Student> students) {
        StringBuilder report = new StringBuilder();
        report.append("\n========== CLASS PERFORMANCE REPORT ==========\n");
        report.append("Total Students: ").append(students.size()).append("\n\n");
        
        GradeService gradeService = new GradeService();
        report.append(String.format("Class Average: %.2f\n", gradeService.calculateClassAverage(students)));
        report.append(String.format("Top Performer: %s (GPA: %.2f)\n", 
            gradeService.findTopPerformer(students).getName(),
            gradeService.findTopPerformer(students).calculateGPA()));
        
        List<Student> atRisk = gradeService.findAtRiskStudents(students);
        report.append(String.format("At-Risk Students: %d\n", atRisk.size()));
        
        report.append("============================================\n\n");
        return report.toString();
    }
    
    public boolean exportReportToFile(String report, String filename) {
        try (FileWriter writer = new FileWriter(filename)) {
            writer.write(report);
            System.out.println("Report exported to " + filename);
            return true;
        } catch (IOException e) {
            System.out.println("Error exporting report: " + e.getMessage());
            return false;
        }
    }
}
```

## 9. ValidationUtil.java

```java
public class ValidationUtil {
    
    public static boolean isValidGrade(double grade) {
        return grade >= 0 && grade <= 100;
    }
    
    public static boolean isValidEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }
    
    public static boolean isValidPassword(String password) {
        return password != null && password.length() >= 6;
    }
    
    public static boolean isValidName(String name) {
        return name != null && !name.isEmpty() && name.length() >= 2;
    }
    
    public static boolean isValidID(int id) {
        return id > 0 && id < 999999;
    }
}
```

## 10. Application.java (Main Class)

```java
import java.util.Scanner;

public class Application {
    
    private static StudentService studentService;
    private static AuthenticationService authService;
    private static GradeService gradeService;
    private static ReportService reportService;
    
    public static void main(String[] args) {
        initializeSystem();
        runMainMenu();
    }
    
    private static void initializeSystem() {
        studentService = new StudentService();
        authService = new AuthenticationService();
        gradeService = new GradeService();
        reportService = new ReportService();
        
        // Add sample data
        studentService.registerStudent(101, "Aryaman Joshi", "aryaman@vitbhopal.ac.in", "pass123");
        studentService.registerAdmin(201, "Dr. Admin", "admin@vitbhopal.ac.in", "admin@123");
    }
    
    private static void runMainMenu() {
        Scanner sc = new Scanner(System.in);
        boolean running = true;
        
        while (running) {
            System.out.println("\n╔════════════════════════════════════════╗");
            System.out.println("║  STUDENT GRADE MANAGEMENT SYSTEM v1.0  ║");
            System.out.println("║      VIT Bhopal University             ║");
            System.out.println("╠════════════════════════════════════════╣");
            System.out.println("║  1. Login as Student                   ║");
            System.out.println("║  2. Login as Administrator             ║");
            System.out.println("║  3. Register New Student               ║");
            System.out.println("║  4. Exit                               ║");
            System.out.println("╚════════════════════════════════════════╝");
            System.out.print("Enter your choice (1-4): ");
            
            try {
                int choice = sc.nextInt();
                sc.nextLine();
                
                switch (choice) {
                    case 1:
                        studentLogin(sc);
                        break;
                    case 2:
                        adminLogin(sc);
                        break;
                    case 3:
                        registerNewStudent(sc);
                        break;
                    case 4:
                        running = false;
                        System.out.println("Thank you for using SGMS. Goodbye!");
                        break;
                    default:
                        System.out.println("Invalid choice! Please try again.");
                }
            } catch (Exception e) {
                System.out.println("Error: Invalid input!");
                sc.nextLine();
            }
        }
        sc.close();
    }
    
    private static void studentLogin(Scanner sc) {
        System.out.print("Enter email: ");
        String email = sc.nextLine();
        System.out.print("Enter password: ");
        String password = sc.nextLine();
        
        Student student = authService.authenticate(studentService, email, password);
        if (student != null && student.getRole().equals("STUDENT")) {
            RegularUser user = (RegularUser) student;
            studentMenu(user, sc);
        }
    }
    
    private static void adminLogin(Scanner sc) {
        System.out.print("Enter email: ");
        String email = sc.nextLine();
        System.out.print("Enter password: ");
        String password = sc.nextLine();
        
        Student student = authService.authenticate(studentService, email, password);
        if (student != null && student.getRole().equals("ADMIN")) {
            AdminUser admin = (AdminUser) student;
            adminMenu(admin, sc);
        }
    }
    
    private static void registerNewStudent(Scanner sc) {
        try {
            System.out.print("Enter Student ID: ");
            int id = sc.nextInt();
            sc.nextLine();
            
            System.out.print("Enter Name: ");
            String name = sc.nextLine();
            
            System.out.print("Enter Email: ");
            String email = sc.nextLine();
            
            System.out.print("Enter Password: ");
            String password = sc.nextLine();
            
            if (ValidationUtil.isValidID(id) && ValidationUtil.isValidName(name) && 
                ValidationUtil.isValidEmail(email) && ValidationUtil.isValidPassword(password)) {
                studentService.registerStudent(id, name, email, password);
            } else {
                System.out.println("Invalid input data!");
            }
        } catch (Exception e) {
            System.out.println("Registration error: " + e.getMessage());
        }
    }
    
    private static void studentMenu(RegularUser student, Scanner sc) {
        boolean inMenu = true;
        while (inMenu) {
            System.out.println("\n=== STUDENT MENU ===");
            System.out.println("1. View My Grades");
            System.out.println("2. View My Performance");
            System.out.println("3. Generate Report");
            System.out.println("4. Logout");
            System.out.print("Enter choice: ");
            
            int choice = sc.nextInt();
            sc.nextLine();
            
            switch (choice) {
                case 1:
                    student.viewOwnGrades();
                    break;
                case 2:
                    student.viewOwnPerformance();
                    break;
                case 3:
                    String report = reportService.generateStudentReport(student);
                    System.out.println(report);
                    break;
                case 4:
                    inMenu = false;
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        }
    }
    
    private static void adminMenu(AdminUser admin, Scanner sc) {
        boolean inMenu = true;
        while (inMenu) {
            System.out.println("\n=== ADMIN MENU ===");
            System.out.println("1. Add Grade");
            System.out.println("2. Update Grade");
            System.out.println("3. Delete Grade");
            System.out.println("4. View Student Details");
            System.out.println("5. Logout");
            System.out.print("Enter choice: ");
            
            int choice = sc.nextInt();
            sc.nextLine();
            
            switch (choice) {
                case 1:
                    addGradeAdmin(admin, sc);
                    break;
                case 2:
                    updateGradeAdmin(admin, sc);
                    break;
                case 3:
                    deleteGradeAdmin(admin, sc);
                    break;
                case 4:
                    viewStudentAdmin(admin, sc);
                    break;
                case 5:
                    inMenu = false;
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        }
    }
    
    private static void addGradeAdmin(AdminUser admin, Scanner sc) {
        System.out.print("Enter Student ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        
        Student student = studentService.getStudentById(id);
        if (student != null) {
            System.out.print("Enter Subject: ");
            String subject = sc.nextLine();
            System.out.print("Enter Grade (0-100): ");
            double grade = sc.nextDouble();
            
            if (ValidationUtil.isValidGrade(grade)) {
                Grade g = new Grade(subject, grade);
                admin.manageStudentGrades(student, "add", subject, grade);
            } else {
                System.out.println("Invalid grade value!");
            }
        } else {
            System.out.println("Student not found!");
        }
    }
    
    private static void updateGradeAdmin(AdminUser admin, Scanner sc) {
        System.out.print("Enter Student ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        
        Student student = studentService.getStudentById(id);
        if (student != null) {
            System.out.print("Enter Subject: ");
            String subject = sc.nextLine();
            System.out.print("Enter New Grade (0-100): ");
            double grade = sc.nextDouble();
            
            if (ValidationUtil.isValidGrade(grade)) {
                admin.manageStudentGrades(student, "update", subject, grade);
            }
        }
    }
    
    private static void deleteGradeAdmin(AdminUser admin, Scanner sc) {
        System.out.print("Enter Student ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        
        Student student = studentService.getStudentById(id);
        if (student != null) {
            System.out.print("Enter Subject: ");
            String subject = sc.nextLine();
            admin.manageStudentGrades(student, "delete", subject, 0);
        }
    }
    
    private static void viewStudentAdmin(AdminUser admin, Scanner sc) {
        System.out.print("Enter Student ID: ");
        int id = sc.nextInt();
        
        Student student = studentService.getStudentById(id);
        if (student != null) {
            admin.viewAllStudentDetails(student);
        } else {
            System.out.println("Student not found!");
        }
    }
}
```

---

**End of Java Source Code**