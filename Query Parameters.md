Here's a **step-by-step tutorial** on how to use query parameters with Next.js's `router.push`, including how to add headers for requests. I’ll keep the explanation beginner-friendly.

---

### 🧠 **What Are Query Parameters?**
Query parameters are key-value pairs added to a URL after a `?`. For example:

```
http://example.com/page?key=value
```

- `?` starts the query string.
- `key=value` is a key-value pair.
- Multiple parameters are separated by `&`:  
  `http://example.com/page?name=John&role=admin`

Query parameters allow you to pass information between pages.
<img src="https://github.com/dsdebnath4663/react-tutorial/blob/main/QueryParameters.png" alt="QueryParameters"/>

---

### 🌟 **Goal**:  
We'll create two pages:
1. A **source page** with a button to navigate to another page with query parameters.
2. A **destination page** to display the query parameters sent.

We'll also make an API call with a custom **header** on the destination page.

---

### 🛠 **Step-by-Step Tutorial**

#### 1️⃣ **Install Next.js Project**
Make sure you have a Next.js app ready. If not, create one:
```bash
npx create-next-app my-app
cd my-app
npm run dev
```

---

#### 2️⃣ **Source Page (`/page1`)**
Create a page where you navigate to another page with query parameters.

**`app/page1/page.js`**:
```jsx
'use client';

import { useRouter } from 'next/navigation';

export default function Page1() {
  const router = useRouter();

  const handleNavigate = () => {
    const id = 2; // Example: Candidate ID
    const name = 'John Doe'; // Example: Candidate name

    router.push(`/candidate/candidate-evaluation?id=${id}&name=${encodeURIComponent(name)}`);
  };

  return (
    <div>
      <h1>Page 1: Navigate with Query Parameters</h1>
      <button onClick={handleNavigate}>Go to Candidate Evaluation</button>
    </div>
  );
}
```

**What’s Happening Here?**
- `router.push`: Navigates to `/candidate/candidate-evaluation` with `id` and `name` as query parameters.
- `encodeURIComponent`: Ensures the query parameter value is URL-safe (e.g., spaces are replaced with `%20`).

---

#### 3️⃣ **Destination Page (`/candidate/candidate-evaluation`)**
On this page, we’ll:
1. Read the query parameters.
2. Make an API request using the query parameter (`id`) and send a custom header.

**`app/candidate/candidate-evaluation/page.js`**:
```jsx
'use client';

import { useSearchParams } from 'next/navigation';
import { useEffect, useState } from 'react';
import axios from 'axios';

export default function CandidateEvaluation() {
  const searchParams = useSearchParams();
  const id = searchParams.get('id'); // Get `id` from query params
  const name = searchParams.get('name'); // Get `name` from query params

  const [candidateData, setCandidateData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch data from API
    const fetchCandidateData = async () => {
      try {
        const response = await axios.get(`/api/candidates/${id}`, {
          headers: {
            Authorization: 'Bearer your-auth-token', // Example: Custom Header
          },
        });
        setCandidateData(response.data);
      } catch (err) {
        setError(err.message);
      }
    };

    if (id) fetchCandidateData();
  }, [id]);

  if (error) return <p>Error: {error}</p>;
  if (!candidateData) return <p>Loading...</p>;

  return (
    <div>
      <h1>Candidate Evaluation</h1>
      <p>Candidate ID: {id}</p>
      <p>Candidate Name: {name}</p>
      <h2>Details from API:</h2>
      <pre>{JSON.stringify(candidateData, null, 2)}</pre>
    </div>
  );
}
```

**What’s Happening Here?**
1. **`useSearchParams`:** Retrieves `id` and `name` from the URL query string.
2. **`axios.get`:** Makes an API call to `/api/candidates/${id}`.
3. **Custom Header:** Adds an `Authorization` header (`Bearer your-auth-token`).
4. **`useEffect`:** Runs the fetch function when the page loads.

---

#### 4️⃣ **Result**
1. Start the server:
   ```bash
   npm run dev
   ```
2. Navigate to `/page1`. Click the button. The URL will change to something like:
   ```
   http://localhost:3000/candidate/candidate-evaluation?id=2&name=John%20Doe
   ```
3. On the destination page:
   - The query parameters (`id` and `name`) are displayed.
   - Data fetched from the API is shown below.

---

### 💡 **Key Concepts to Remember**

1. **Query Parameters:**
   - Pass small data through the URL.
   - Use `useSearchParams` to extract them in Next.js.

2. **Custom Headers in API Calls:**
   - Add headers to requests for authentication or extra metadata:
     ```javascript
     axios.get(url, { headers: { Authorization: 'Bearer token' } });
     ```

3. **`router.push`:**
   - Use `router.push('/path?key=value')` for navigation with query parameters.

---

### 🌟 Example Output

#### **Source Page:**
```
Page 1: Navigate with Query Parameters
[Button: Go to Candidate Evaluation]
```

#### **Destination Page:**
```
Candidate Evaluation
Candidate ID: 2
Candidate Name: John Doe
Details from API:
{
  "id": 2,
  "name": "John Doe",
  "role": "Software Engineer",
  "experience": "5 years"
}
```

---

That's it! Let me know if you need more details! 😊
