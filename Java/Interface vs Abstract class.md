
## Interface vs Abstract class

---

### **Think of it Like a Job Description vs. an Employee Template**

Imagine you’re in charge of hiring staff for a hospital, and you need two things:

1. **Job Descriptions** (what roles should do)
2. **Employee Templates** (what kind of employees you’re hiring and what they should already have)

Now, let’s connect this idea to **interfaces** and **abstract classes**.

---

### **1. Interface (Job Description)**

An **interface** is like writing a **Job Description** for a role in the hospital. It tells you **what tasks** someone needs to do, but **doesn’t tell you how** to do them. You just say, "This is what the job requires."

For example, if you want to define a role for **Healthcare Professionals** (Doctors, Nurses, Therapists), the interface would list the tasks like:

- **Diagnose patients**
- **Treat patients**

So, you could have:

- **Doctor** implements this job description and says, "I diagnose and treat patients in my way."
- **Nurse** implements the same job description and says, "I assist in diagnosing and provide medication in my way."

The key point is that both must **follow the same job description** (interface) but can **do things their own way**.

**Example in HIMS:**
```java
interface HealthcareProfessional {
    void diagnosePatient();
    void treatPatient();
}
```
- A **Doctor** and a **Nurse** would both follow this job description but do things differently. The **Doctor** might diagnose by using medical tests, while the **Nurse** might assist in diagnosis but also help with treatments and care.

### **When to Use an Interface:**

- **When you need multiple roles to do similar tasks but in different ways**.
- **When you don’t need to share common details** (like a salary or name) but only care about the tasks they do.
- **When you need multiple people (classes) to follow the same set of rules** without worrying about the specifics of how they do it.

---

### **2. Abstract Class (Employee Template)**

An **abstract class** is like creating an **Employee Template**. This is like saying, "All employees will have some basic details (name, salary), and they will need to do their jobs in certain ways, but some things can be different based on the type of employee."

For example, let’s say you have a hospital **Employee Template**:
- All employees have a **name** and **salary**.
- All employees must **perform a duty**, but what they do (e.g., treating a patient or checking records) depends on their role.

In this case, you could create an **Employee** class as an abstract class, which contains some basic details (name, salary) and a method to show information (`displayInfo()`), but leaves the specific duties (like **treating patients**) for subclasses to define.

**Example in HIMS:**
```java
abstract class Employee {
    String name;
    double salary;

    // Constructor to set the name and salary
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // A shared method
    public void displayInfo() {
        System.out.println("Employee: " + name + ", Salary: " + salary);
    }

    // Abstract method - must be defined by subclasses
    abstract void performDuties();
}
```
- A **Doctor** will say, "I diagnose and treat patients."
- A **Nurse** might say, "I assist doctors and take care of patients."

The **Employee** class gives them some shared things (name, salary), but the specific duties (what the doctor or nurse does) are left for them to define.

### **When to Use an Abstract Class:**

- **When employees (or objects) share common properties** (like name, salary, etc.) but also need some things to be **specific to each role** (like treating patients for a doctor or assisting for a nurse).
- **When you want to give some shared functionality** to all roles, but allow specific roles to define their own behavior.
- **When the base class needs to define some methods that are common for all employees, but also leave room for customization** (like `displayInfo()`).

---

### **Summary for HIMS System:**

- **Use an Interface (Job Description)** if you want to define **common tasks** (like diagnosing or treating patients) that different people (doctors, nurses, therapists) will do in their own ways. The interface doesn’t care how they do it, just that they do it.

    - Example: All healthcare professionals should be able to **diagnose** and **treat**, but they each may do it differently.

- **Use an Abstract Class (Employee Template)** if you want to define **common details** for all employees (like their name and salary) but still allow them to have different jobs or duties (doctors diagnose, nurses assist). This gives you both shared details and flexibility for different tasks.

    - Example: All hospital staff (doctors, nurses) will have **name** and **salary**, but they’ll perform different duties like diagnosing or assisting patients.

---

### **Real-World Analogy:**

- **Interface:** Think of it like a **job listing** for "Healthcare Professional" – it lists the tasks (e.g., diagnose patients, treat patients), but it doesn’t care how you do them. Any healthcare worker can follow this job description, but they do it in their own way.

- **Abstract Class:** Think of it like a **generic employee template** where all employees (Doctors, Nurses) have certain things in common (like a name and salary), but each employee does different duties based on their role. The **Employee** class gives you common info, but subclasses (Doctor, Nurse) provide specific actions.

---

**In Short:**
- Use **interfaces** to **define common tasks** (like diagnosing patients) across many classes (doctors, nurses) without caring about how they do it.
- Use **abstract classes** when you want to define **shared characteristics and common functionality** (like employee name, salary) but also allow flexibility for specific details (like what each employee does in their job).

Sure! Below are **full examples** for each case, using the **Hospital Information Management System (HIMS)** analogy for a **HealthcareProfessional** (interface) and **Employee** (abstract class).

### **1. Interface Example:**

In this case, we’ll use an interface to define the **common tasks** that different healthcare professionals (Doctors, Nurses, Therapists) should perform. Each role will implement the interface, but each one can perform these tasks in its own way.

#### **Interface: HealthcareProfessional**

```java
// Interface defining the common tasks for healthcare professionals
interface HealthcareProfessional {
    void diagnosePatient();  // Each healthcare professional must diagnose a patient
    void treatPatient();     // Each healthcare professional must treat a patient
}
```

#### **Doctor Class** (implements the `HealthcareProfessional` interface)

```java
// Doctor class implementing the HealthcareProfessional interface
class Doctor implements HealthcareProfessional {
    private String name;

    public Doctor(String name) {
        this.name = name;
    }

    @Override
    public void diagnosePatient() {
        System.out.println(name + " is diagnosing the patient using medical tests.");
    }

    @Override
    public void treatPatient() {
        System.out.println(name + " is prescribing medication to treat the patient.");
    }
}
```

#### **Nurse Class** (implements the `HealthcareProfessional` interface)

```java
// Nurse class implementing the HealthcareProfessional interface
class Nurse implements HealthcareProfessional {
    private String name;

    public Nurse(String name) {
        this.name = name;
    }

    @Override
    public void diagnosePatient() {
        System.out.println(name + " is assisting with patient diagnosis and taking vital signs.");
    }

    @Override
    public void treatPatient() {
        System.out.println(name + " is administering prescribed medication and caring for patients.");
    }
}
```

#### **Main Class** (to test the Interface)

```java
public class HIMSInterfaceExample {
    public static void main(String[] args) {
        HealthcareProfessional doctor = new Doctor("Dr. John");
        HealthcareProfessional nurse = new Nurse("Nurse Anna");

        // Doctor performing tasks
        doctor.diagnosePatient();
        doctor.treatPatient();

        // Nurse performing tasks
        nurse.diagnosePatient();
        nurse.treatPatient();
    }
}
```

### **Output:**
```
Dr. John is diagnosing the patient using medical tests.
Dr. John is prescribing medication to treat the patient.
Nurse Anna is assisting with patient diagnosis and taking vital signs.
Nurse Anna is administering prescribed medication and caring for patients.
```

#### **Explanation of Interface Example:**
- **HealthcareProfessional** is an interface that defines the common behavior (diagnosing and treating patients).
- **Doctor** and **Nurse** implement this interface, each providing their own implementation for the methods `diagnosePatient()` and `treatPatient()`.
- The interface ensures that all healthcare professionals can perform these tasks, but each class decides how it will perform them.

---

### **2. Abstract Class Example:**

In this case, we’ll use an **abstract class** to define common properties and behavior (like `name` and `salary`) for all employees, but leave specific duties (like `performDuties()`) to be implemented by subclasses.

#### **Abstract Class: Employee**

```java
// Abstract class defining common properties and methods for all hospital employees
abstract class Employee {
    String name;
    double salary;

    // Constructor to initialize name and salary
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // Concrete method: Display basic employee info
    public void displayInfo() {
        System.out.println("Employee: " + name + ", Salary: " + salary);
    }

    // Abstract method: Specific duties will be defined by subclasses
    abstract void performDuties();
}
```

#### **Doctor Class** (extends `Employee` and implements `performDuties`)

```java
// Doctor class extending Employee and providing specific duties
class Doctor extends Employee {
    public Doctor(String name, double salary) {
        super(name, salary);
    }

    @Override
    void performDuties() {
        System.out.println(name + " is diagnosing and treating patients.");
    }
}
```

#### **Nurse Class** (extends `Employee` and implements `performDuties`)

```java
// Nurse class extending Employee and providing specific duties
class Nurse extends Employee {
    public Nurse(String name, double salary) {
        super(name, salary);
    }

    @Override
    void performDuties() {
        System.out.println(name + " is assisting doctors and providing patient care.");
    }
}
```

#### **Main Class** (to test the Abstract Class)

```java
public class HIMSAbstractClassExample {
    public static void main(String[] args) {
        Employee doctor = new Doctor("Dr. Smith", 100000);
        Employee nurse = new Nurse("Nurse Sarah", 50000);

        // Display basic employee info
        doctor.displayInfo();
        doctor.performDuties();

        nurse.displayInfo();
        nurse.performDuties();
    }
}
```

### **Output:**
```
Employee: Dr. Smith, Salary: 100000.0
Dr. Smith is diagnosing and treating patients.
Employee: Nurse Sarah, Salary: 50000.0
Nurse Sarah is assisting doctors and providing patient care.
```

#### **Explanation of Abstract Class Example:**
- **Employee** is an abstract class that provides some shared properties (like `name` and `salary`) and methods (like `displayInfo()`), but leaves the `performDuties()` method to be defined by subclasses.
- **Doctor** and **Nurse** classes extend `Employee` and provide their specific implementation of the `performDuties()` method, while inheriting common properties like `name` and `salary`.

---

### **Summary of Differences:**

| **Feature**               | **Interface (Job Description)**                                  | **Abstract Class (Employee Template)**                          |
|---------------------------|-------------------------------------------------------------------|------------------------------------------------------------------|
| **Purpose**                | Defines a contract for behavior (tasks) without implementation.  | Defines common properties and behaviors, allowing customization.|
| **Methods**                | Can only have abstract methods (unless default methods are used).| Can have both abstract methods (to be implemented) and concrete methods (with behavior).|
| **Multiple Inheritance**   | Can implement multiple interfaces.                               | Can extend only one abstract class.                             |
| **Fields**                 | Cannot have instance variables (only constants).                 | Can have instance variables (e.g., name, salary).               |
| **Use Case**               | When you need multiple classes to do the same type of work but in different ways (no shared state). | When you want to define shared properties (like `name`, `salary`) and methods that multiple subclasses can inherit. |

---

### **When to Use Which:**
- **Use an interface** when you want to define a **set of tasks** that different roles (like doctors and nurses) must follow, but each role does it in their own way (no shared state).
- **Use an abstract class** when you want to define some **shared properties and methods** (like name and salary) for employees in the hospital, but still allow each role (doctor, nurse) to implement their specific tasks.

