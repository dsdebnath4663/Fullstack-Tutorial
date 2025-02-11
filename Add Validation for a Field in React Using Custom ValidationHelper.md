Here's a **beginner-friendly documentation** on how to **add validation for a field** in your React form using the **ValidationHelper** utility.

---

# **How to Add Validation for a Field in React Using ValidationHelper**
This guide will help beginners understand how to add validation for a field in a React form using a validation helper utility.

---

## **Step 1: Understanding the ValidationHelper**
The **ValidationHelper** is a JavaScript object that provides functions to validate form fields. It includes:
1. `validateField(fieldName, value, type)`: Checks if a field is valid and returns an error message if invalid.
2. `getValidationClass(fieldName, touchedFields, formErrors)`: Returns `is-valid` or `is-invalid` based on validation.
3. `getFeedbackClass(fieldName, touchedFields, formErrors)`: Returns feedback class (`valid-feedback` or `invalid-feedback`).
4. `getFeedbackMessage(fieldName, touchedFields, formErrors)`: Returns the appropriate validation message.

---

## **Step 2: Define the Form State**
First, define the **state variables** to store form data, errors, and touched fields.

```jsx
import React, { useState } from "react";
import { ValidationHelper } from "@/components/form-validator/ValidationHelper";

const MyForm = () => {
  const [formData, setFormData] = useState({
    firstName: "",
    email: "",
  });

  const [formErrors, setFormErrors] = useState({});
  const [touchedFields, setTouchedFields] = useState({});

  return <div>Form Component</div>;
};

export default MyForm;
```

---

## **Step 3: Create an Input Field with Validation**
To add validation for a field (e.g., `firstName`):

1. **Update the State on Input Change**  
   - Use `handleInputChange` to track input changes and validate fields.
   - Use `setTouchedFields` to mark a field as touched.

```jsx
const handleInputChange = (e) => {
  const { name, value } = e.target;

  setFormData((prev) => ({ ...prev, [name]: value }));
  setTouchedFields((prev) => ({ ...prev, [name]: true }));

  // Validate the field using ValidationHelper
  const error = ValidationHelper.validateField(name, value, typeof value);
  setFormErrors((prev) => ({ ...prev, [name]: error }));
};
```

---

## **Step 3.1: Add  Input Fields in ValidationHelper.js to validate**

Here's the updated **required validation fields** section with comments explaining when to use each validation type:

```javascript
const requiredTextFields = [
  // Fields that require non-empty text input (Strings)

];

// Fields that require numeric values (Only numbers are valid)
const requiredNumericFields = [
  // Use this for fields where the value should be a positive number (e.g., salary, years of experience)
];

// Fields that require file uploads (Should not be empty)
const requiredFileFields = [
  // Use this for file input fields where users must upload a document (e.g., resume, offer letters)
];

// Fields that require date inputs (Should not be empty)
const requiredDateFields = [
  // Use this for fields where users must select a valid date (e.g., job opening date, target completion date)
];
```

---

# **When to Use Each Validation Type?**
- **`requiredTextFields`** → Use for any input that **must contain text** (e.g., Name, Email, Address, Job Title).
- **`requiredNumericFields`** → Use for **numeric inputs only** (e.g., Salary, Years of Experience, Number of Positions).
- **`requiredFileFields`** → Use for **file uploads** (e.g., Resume, Offer Letter, Contract).
- **`requiredDateFields`** → Use for **date inputs** (e.g., Opening Date, Target Completion Date).

This ensures **better data integrity and user experience** by validating fields correctly. 🚀
---
## **Step 4: Display Validation Messages**
Use the **helper functions** to show validation styles and messages in the form.

```jsx
<form>
  {/* First Name Field */}
  <label htmlFor="firstName">First Name:</label>
  <input
    type="text"
    name="firstName"
    id="firstName"
    className={`form-control ${ValidationHelper.getValidationClass("firstName", touchedFields, formErrors)}`}
    value={formData.firstName}
    onChange={handleInputChange}
  />
  <div className={ValidationHelper.getFeedbackClass("firstName", touchedFields, formErrors)}>
    {ValidationHelper.getFeedbackMessage("firstName", touchedFields, formErrors)}
  </div>

  {/* Email Field */}
  <label htmlFor="email">Email:</label>
  <input
    type="email"
    name="email"
    id="email"
    className={`form-control ${ValidationHelper.getValidationClass("email", touchedFields, formErrors)}`}
    value={formData.email}
    onChange={handleInputChange}
  />
  <div className={ValidationHelper.getFeedbackClass("email", touchedFields, formErrors)}>
    {ValidationHelper.getFeedbackMessage("email", touchedFields, formErrors)}
  </div>

  <button type="submit">Submit</button>
</form>
```

---

## **Step 5: Handle Form Submission**
Ensure all fields are validated before submitting the form.

```jsx

  const handleSubmit = (e) => {
    e.preventDefault();
    let valid = true;
    const newErrors = {};
    const touched = {};

    if (!formData) {
      console.error("❌ formData is undefined");
      alert("❌ formData is undefined");
      return;
    }
    Object.keys(formData).forEach((field) => {
      if (formData[field] === undefined || formData[field] === null) {
        console.warn(`Field ${field} is undefined/null`);
        alert(`❌ Field ${field} is undefined/null`);
      }
      const error = ValidationHelper.validateField(field, formData[field], typeof formData[field]);
      if (error) valid = false;
      newErrors[field] = error;

      touched[field] = true;
    });

    setFormErrors(newErrors);
    setTouchedFields(touched);

    if (valid) {
      console.log("✅ All input fields are filled. Submitting form...", formData);
    } else {
      console.log(
        "❌ Form has missing inputs fields. Fix errors before submitting."
      );
      alert("⚠️ Some  input fields are missing. Please fill them in.");
      console.log("Form has validation errors.");
    }
  }
```

### Alternate

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  let valid = true;
  const newErrors = {};
  const touched = {};

  Object.keys(formData).forEach((field) => {
    const error = ValidationHelper.validateField(field, formData[field], typeof formData[field]);
    if (error) valid = false;
    newErrors[field] = error;
    touched[field] = true;
  });

  setFormErrors(newErrors);
  setTouchedFields(touched);

  if (valid) {
    console.log("Form submitted successfully!", formData);
  } else {
    console.log("Form has validation errors.");
  }
};
```

---

## **Step 6: Add the Submit Button**
Attach `handleSubmit` to the form submit button.

```jsx
<button type="submit" onClick={handleSubmit} className="btn btn-primary">
  Submit
</button>
```

---

# **Final Code (Complete Example)**
Here’s the complete implementation of a form with validation.

```jsx
import React, { useState } from "react";
import { ValidationHelper } from "@/components/form-validator/ValidationHelper";

const MyForm = () => {
  const [formData, setFormData] = useState({
    firstName: "",
    email: "",
  });

  const [formErrors, setFormErrors] = useState({});
  const [touchedFields, setTouchedFields] = useState({});

  const handleInputChange = (e) => {
    const { name, value } = e.target;

    setFormData((prev) => ({ ...prev, [name]: value }));
    setTouchedFields((prev) => ({ ...prev, [name]: true }));

    const error = ValidationHelper.validateField(name, value, typeof value);
    setFormErrors((prev) => ({ ...prev, [name]: error }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    let valid = true;
    const newErrors = {};
    const touched = {};

    Object.keys(formData).forEach((field) => {
      const error = ValidationHelper.validateField(field, formData[field], typeof formData[field]);
      if (error) valid = false;
      newErrors[field] = error;
      touched[field] = true;
    });

    setFormErrors(newErrors);
    setTouchedFields(touched);

    if (valid) {
      console.log("Form submitted successfully!", formData);
    } else {
      console.log("Form has validation errors.");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="firstName">First Name:</label>
      <input
        type="text"
        name="firstName"
        id="firstName"
        className={`form-control ${ValidationHelper.getValidationClass("firstName", touchedFields, formErrors)}`}
        value={formData.firstName}
        onChange={handleInputChange}
      />
      <div className={ValidationHelper.getFeedbackClass("firstName", touchedFields, formErrors)}>
        {ValidationHelper.getFeedbackMessage("firstName", touchedFields, formErrors)}
      </div>

      <label htmlFor="email">Email:</label>
      <input
        type="email"
        name="email"
        id="email"
        className={`form-control ${ValidationHelper.getValidationClass("email", touchedFields, formErrors)}`}
        value={formData.email}
        onChange={handleInputChange}
      />
      <div className={ValidationHelper.getFeedbackClass("email", touchedFields, formErrors)}>
        {ValidationHelper.getFeedbackMessage("email", touchedFields, formErrors)}
      </div>

      <button type="submit" className="btn btn-primary">
        Submit
      </button>
    </form>
  );
};

export default MyForm;
```

---

# **Conclusion**
- **Step 1**: Define form state using `useState`.
- **Step 2**: Implement `handleInputChange` to track changes and validate fields.
- **Step 3**: Display validation messages using `ValidationHelper` methods.
- **Step 4**: Handle form submission and prevent submission if errors exist.

With this approach, you can **easily add validation for any field** by updating the `ValidationHelper` and adding new fields in the form. 🚀
