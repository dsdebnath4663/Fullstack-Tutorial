Below is a step-by-step tutorial for beginners on how to add a modal component (like `CandidateSelectionModal`) and update the parent component’s state (in `InterviewFormHandler`) with the selected value.

---

## **Step-by-Step Tutorial**

### **Step 1: Create the Modal Component (`CandidateSelectionModal`)**

1. **Create a New File:**

   Create a new file named `CandidateSelectionModal.jsx`.

2. **Import Dependencies and Set Up the Component:**

   At the top of your file, import React and any hooks you need:
   
   ```jsx
   "use client";
   import React, { useState, useEffect } from "react";
   import { fetchAllCandidates } from "@/utils/restClient"; // Assumes this function fetches candidates
   ```

3. **Define the Component and Set Default Props:**

   Define the component to accept props for initial data, closing the modal, and saving the selection. Use a default empty object for `initialData` so that you always have defined values.
   
   ```jsx
   export default function CandidateSelectionModal({
     initialData = {},
     onClose,
     onSave,
   }) {
     // Initialize local state for candidate selection.
     const [formData, setFormData] = useState({
       candidateId: initialData.candidateId || "",
       candidateName: initialData.candidateName || "",
     });
   ```

4. **Fetch Candidate Data and Set Up Local States:**

   Use the `useEffect` hook to fetch candidates when the component mounts. Also, include state for search and filtered candidates.
   
   ```jsx
     const [candidates, setCandidates] = useState([]);
     const [searchQuery, setSearchQuery] = useState("");
     const [filteredCandidates, setFilteredCandidates] = useState([]);
   
     useEffect(() => {
       async function fetchCandidatesData() {
         try {
           const response = await fetchAllCandidates();
           const candidatesList = response.map((candidate) => ({
             id: candidate.id,
             fullName: `${candidate.firstName} ${candidate.lastName}`,
           }));
           setCandidates(candidatesList);
           setFilteredCandidates(candidatesList);
         } catch (error) {
           console.error("Failed to fetch candidates:", error);
           setCandidates([]);
           setFilteredCandidates([]);
         }
       }
       fetchCandidatesData();
     }, []);
   ```

5. **Filter Candidates Based on Search Input:**

   Use another `useEffect` to filter candidates when the search query changes.
   
   ```jsx
     useEffect(() => {
       if (searchQuery) {
         const filtered = candidates.filter((candidate) =>
           candidate.fullName.toLowerCase().includes(searchQuery.toLowerCase())
         );
         setFilteredCandidates(filtered);
       } else {
         setFilteredCandidates(candidates);
       }
     }, [searchQuery, candidates]);
   ```

6. **Handle Input Changes:**

   Write a change handler that updates the local state when the user selects a candidate:
   
   ```jsx
     const handleChange = (e) => {
       const { name, value } = e.target;
       if (name === "candidateId") {
         const selectedCandidate = candidates.find(
           (candidate) => String(candidate.id) === value
         );
         setFormData({
           ...formData,
           candidateId: value,
           candidateName: selectedCandidate ? selectedCandidate.fullName : "",
         });
       } else {
         setFormData({ ...formData, [name]: value });
       }
     };
   ```

7. **Handle Search Input Changes:**

   Update the search query when the user types:
   
   ```jsx
     const handleSearchChange = (e) => {
       setSearchQuery(e.target.value);
     };
   ```

8. **Save the Selected Candidate:**

   When the user clicks the "Save" button, pass the selected candidate data back to the parent and close the modal:
   
   ```jsx
     const handleSave = (e) => {
       e.preventDefault();
       onSave(formData); // Pass the selected candidate data to the parent.
       onClose();       // Close the modal.
     };
   ```

9. **Render the Modal:**

   Finally, render the modal with a list of candidates and controlled radio inputs.
   
   ```jsx
     return (
       <div className="modal show" style={{ display: "block" }} tabIndex="-1" role="dialog">
         <div className="modal-dialog modal-xl" role="document">
           <div className="modal-content">
             <div className="modal-header">
               <h5 className="modal-title">Select Candidate :</h5>
               <div className="position-relative ms-auto">
                 <input
                   className="form-control ps-5 bg-light"
                   type="search"
                   placeholder="Search..."
                   value={searchQuery}
                   onChange={handleSearchChange}
                 />
                 <button className="btn bg-transparent px-2 py-0 position-absolute top-50 start-0 translate-middle-y" type="submit">
                   <i className="bi bi-search fs-5"></i>
                 </button>
               </div>
               <button type="button" className="btn-md btn btn-primary ms-2">
                 <i className="bi bi-plus-lg me-1"></i>
                 New Candidate
               </button>
               <button type="button" className="btn-close" aria-label="Close" onClick={onClose} />
             </div>
             <div className="modal-body">
               <form onSubmit={handleSave}>
                 <table className="table table-striped">
                   <thead>
                     <tr>
                       <th></th>
                       <th>Candidate Name</th>
                     </tr>
                   </thead>
                   <tbody>
                     {filteredCandidates.map((candidate) => (
                       <tr key={candidate.id}>
                         <th>
                           <input
                             type="radio"
                             name="candidateId"
                             value={candidate.id}
                             checked={String(formData.candidateId) === String(candidate.id)}
                             onChange={handleChange}
                           />
                         </th>
                         <td>{candidate.fullName}</td>
                       </tr>
                     ))}
                   </tbody>
                 </table>
                 <div className="modal-footer">
                   <button type="button" className="btn-sm btn btn-secondary" onClick={onClose}>
                     Close
                   </button>
                   <button type="submit" className="btn-sm btn btn-primary">
                     Save Selection
                   </button>
                 </div>
               </form>
             </div>
           </div>
         </div>
       </div>
     );
   }
   ```

10. **Export the Component:**

    Make sure to export your component:
    
    ```jsx
    export default CandidateSelectionModal;
    ```

---

### **Step 2: Integrate the Modal in the Parent Component (`InterviewFormHandler`)**

1. **Create or Open the Parent File:**

   Open `InterviewFormHandler.jsx` (or create it if it doesn’t exist).

2. **Import React and the Modal Components:**

   At the top of your file, import React, useState, and the modal components:
   
   ```jsx
   "use client";
   import React, { useState } from "react";
   import AddDepartmentModal from "@/components/modal/add-department/add-department";
   import CandidateSelectionModal from "@/components/modal/candidate-selection/CandidateSelectionModal";
   ```

3. **Set Up the Parent Component’s State:**

   Define the state that holds the form data. Make sure to include candidate-related fields:
   
   ```jsx
   const InterviewFormHandler = () => {
     const [formData, setFormData] = useState({
       interviewName: "",
       parentDepartmentId: "",
       departmentName: "",
       from: "",
       interviewers: "",
       location: "",
       candidateId: "",
       candidateName: "",
       postingTitle: "",
       to: "",
       interviewOwner: "",
       scheduleComments: "",
       assessmentName: "",
       reminder: "",
     });
   
     // Data for department modal (if needed)
     const [data, setData] = useState({
       departmentName: "",
       parentDepartmentId: "",
       departmentLead: { firstName: "", lastName: "", email: "" },
       attachmentPath: "",
     });
   
     // Modal visibility state ("department" or "candidate")
     const [showModal, setShowModal] = useState(null);
   ```

4. **Create Functions to Open and Close the Modal:**

   Write functions to open a specific modal and to close the modal:
   
   ```jsx
     const handleOpenModal = (modalType) => {
       setShowModal(modalType);
     };
   
     const handleCloseModal = () => {
       setShowModal(null);
     };
   ```

5. **Create a Function to Save Modal Data:**

   Write a callback function that the modal will call when the user saves their selection. This function updates the parent state:
   
   ```jsx
     const handleSaveData = (updatedData) => {
       setFormData((prevFormData) => ({
         ...prevFormData,
         departmentName: updatedData.departmentName || prevFormData.departmentName,
         parentDepartmentId: updatedData.parentDepartmentId || prevFormData.parentDepartmentId,
         candidateId: updatedData.candidateId || prevFormData.candidateId,
         candidateName: updatedData.candidateName || prevFormData.candidateName,
       }));
     };
   ```

6. **Write a Generic Change Handler for the Form:**

   Include a function to update the parent form state when inputs change:
   
   ```jsx
     const handleChange = (e) => {
       const { name, value, type, checked, files } = e.target;
       setFormData((prevState) => ({
         ...prevState,
         [name]: type === "checkbox" ? checked : type === "file" ? files[0] : value,
       }));
     };
   ```

7. **Write the Form Submission Handler:**

   Create a function to handle form submission:
   
   ```jsx
     const handleSubmit = (e) => {
       e.preventDefault();
       console.log("Form submitted with data:", formData);
     };
   ```

8. **Render the Parent Component and Conditionally Render the Modal:**

   In your JSX, include buttons to open the modal and render the modal only when needed:
   
   ```jsx
     return (
       <div className="container mt-5">
         {showModal === "department" && (
           <AddDepartmentModal
             initialData={data}
             onClose={handleCloseModal}
             onSave={handleSaveData}
           />
         )}
         {showModal === "candidate" && (
           <CandidateSelectionModal
             initialData={formData} // Pass candidate data
             onClose={handleCloseModal}
             onSave={handleSaveData}
           />
         )}
         <div className="card shadow-sm">
           <div className="card-header bg-primary text-white">
             <h3 className="mb-0">Interview Information</h3>
           </div>
           <div className="card-body">
             <form onSubmit={handleSubmit} className="needs-validation" noValidate>
               <div className="row g-3">
                 {/* Interview Name Field */}
                 <div className="col-md-6">
                   <label className="form-label">
                     Interview Name <span className="text-danger fs-5">*</span>
                   </label>
                   <select
                     className="form-select"
                     name="interviewName"
                     value={formData.interviewName}
                     onChange={handleChange}
                     required
                   >
                     <option value="">Select Interview</option>
                     <option value="Technical">Technical</option>
                     <option value="HR">HR</option>
                     <option value="Manager">Manager</option>
                   </select>
                 </div>
   
                 {/* Candidate Name Field with Modal Button */}
                 <div className="col-md-6">
                   <label className="form-label">Candidate Name</label>
                   <div className="position-relative">
                     <input
                       type="text"
                       className="form-control"
                       name="candidateName"
                       value={formData.candidateName || ""}
                       onChange={handleChange}
                     />
                     <button
                       type="button"
                       className="btn btn-light position-absolute top-50 end-0 translate-middle-y"
                       onClick={() => handleOpenModal("candidate")}
                       style={{
                         whiteSpace: "nowrap",
                         height: "33.1px",
                         borderRadius: "0px 3.2px 3.2px 0px",
                       }}
                     >
                       <i className="bi bi-buildings fs-6"></i>
                     </button>
                   </div>
                 </div>
   
                 {/* Additional form fields can be added here */}
   
                 {/* Submit Button */}
                 <div className="col-12">
                   <button type="submit" className="btn btn-primary">
                     Submit
                   </button>
                 </div>
               </div>
             </form>
           </div>
         </div>
       </div>
     );
   };
   
   export default InterviewFormHandler;
   ```

---

### **Step 3: How It All Works Together**

1. **Opening the Modal:**  
   When the user clicks the button next to the Candidate Name input, the `handleOpenModal("candidate")` function is called, updating `showModal` to `"candidate"`. This causes the `CandidateSelectionModal` to be rendered.

2. **Selecting a Candidate:**  
   Inside `CandidateSelectionModal`, the user can search and select a candidate. The modal updates its local state (`formData`) when a radio button is changed.

3. **Saving the Selection:**  
   When the user clicks the "Save Selection" button in the modal, the `handleSave` function is triggered. This calls `onSave(formData)` (which is `handleSaveData` in the parent) to update the parent's form state with the selected candidate, and then calls `onClose()` to hide the modal.

4. **Parent Updates:**  
   The parent component (`InterviewFormHandler`) receives the candidate data through `handleSaveData` and updates its `formData`. The Candidate Name input now reflects the newly selected candidate.

5. **Final Submission:**  
   When the form is submitted in `InterviewFormHandler`, all the collected data—including the candidate information—is available in `formData`.

---

## **Conclusion**

By following these steps:

1. **Create the Modal Component:**  
   Build `CandidateSelectionModal` to fetch candidates, allow searching, and let the user select one candidate using controlled inputs.

2. **Integrate the Modal in the Parent:**  
   In `InterviewFormHandler`, manage modal visibility, pass initial data to the modal, and update the form state when the modal returns new candidate information.

3. **Combine and Test:**  
   Test the entire flow—from opening the modal and selecting a candidate to having the parent’s form state update—ensuring that the component remains controlled and data is passed back and forth correctly.

This step-by-step approach will help you build modals for your application and manage shared state between modals and their parent components. Happy coding!
