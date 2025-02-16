Here’s the improved and formatted version of the beginner-friendly guide for making a REST API call in Next.js:

---

### **Making a REST API Call in Next.js**

In this guide, we'll walk through how to create a function to fetch data from an API, call that function inside your Next.js component, and store the response data in the component’s state.

---

### **Step 1: Create a `restClient.js` File**

First, we need to create a `restClient.js` file that will handle the API request. This file will define functions to fetch data from the API.

**Create a `restClient.js` file** in the root of your project or in a specific `utils` folder.

Define a function to make a **GET** request using **Axios** (or any HTTP client you prefer).

```js
// restClient.js


// Function to fetch job applications statuses
export const fetchJobApplicationsStatus = async () => {
  try {
    const response = await restClient.get('/api/job-applications/statuses');
    return response.data; // Return the response data
  } catch (error) {
    console.error('Error fetching job applications:', error);
    throw error; // Throw the error for further handling
  }
};
export default restClient;
```

---

### **Step 2: Create a State Variable to Store the Response Data in the Corresponding Page (e.g., `CreateCandidateForm`)**

In this step, we’ll create a state variable in your page component where you want to store the fetched data (e.g., `CreateCandidateForm`). This state will hold the response data from the API.

```js
// CreateCandidateForm.js

import React, { useState, useEffect } from 'react';
import { fetchJobApplicationsStatus } from '../utils/restClient'; // Import the function to fetch statuses

const CreateCandidateForm = () => {
  // Initialize the state to store the job application statuses
  const [statusCategories, setStatusCategories] = useState([]);

  // Call fetchJobApplicationsStatus from restClient.js in CreateCandidateForm
  useEffect(() => {
    async function fetchJobApplicationsStatusData() {
      try {
        const response = await fetchJobApplicationsStatus(); // Fetch data from the API
        console.log("Fetched job applications statuses:", response); // Log the response for debugging
        setStatusCategories(response); // Store the response data in state
      } catch (error) {
        setStatusCategories([]); // Set an empty array in case of error
      }
    }

    fetchJobApplicationsStatusData(); // Trigger the API call when the component mounts
  }, []); // The empty array ensures it runs only once when the component mounts

  return (
    <div>
      <h1>Create Candidate Form</h1>
      <div>
        <h2>Status Categories:</h2>
        <ul>
          {statusCategories.length > 0 ? (
            statusCategories.map((status, index) => (
              <li key={index}>{status}</li> // Render each status
            ))
          ) : (
            <li>No statuses available</li> // Display a message if no statuses are available
          )}
        </ul>
      </div>
    </div>
  );
};

export default CreateCandidateForm;
```

---

### **Explanation**

- **State Initialization**:  
  The `statusCategories` state is initialized as an empty array using `useState`. This will store the fetched data from the API.

- **useEffect Hook**:  
  The `useEffect` hook runs when the component first mounts, and inside it, we call the `fetchJobApplicationsStatus` function to fetch the job application statuses.

- **API Call and Error Handling**:  
  The API call is wrapped in a `try-catch` block. If successful, the response data is saved into the `statusCategories` state. If there’s an error, the state is set to an empty array.

- **Rendering Data**:  
  The component checks if there are any statuses in the `statusCategories` array. If there are, it maps over them and displays each status in a list. If there are no statuses, it shows a message indicating that no statuses are available.

---

This approach helps you structure your component logically while ensuring that API data is fetched and displayed efficiently.

