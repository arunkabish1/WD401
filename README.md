## Handling Code Review Feedback
Original Code
     
     const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
     event.preventDefault();
     try {
       const response = await fetch(`${API_ENDPOINT}/organisations`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      // Consider sanitizing and validating the input data before sending it to the API.
      // Updated: Added input sanitization and validation
      body: JSON.stringify({
        name: sanitize(organisationName),
        user_name: sanitize(userName),
        email: validateEmail(userEmail) ? userEmail : throw new Error('Invalid email format'),
        password: userPassword.length >= 8 ? userPassword : throw new Error('Password too short'),
      }),
    });

    if (!response.ok) {
      // Provide more detailed error messages based on response status.
      throw new Error(`Sign-up failed: ${response.statusText}`);
    }
    console.log('Sign-up successful');

    const data = await response.json();
    // Consider using a secure storage mechanism for tokens and sensitive data.
    sessionStorage.setItem('authToken', data.token); // Changed to sessionStorage for security
    sessionStorage.setItem('userData', JSON.stringify(data.user));
    navigate("/dashboard");
    } catch (error) {
    // Implement user-friendly error handling, potentially with a notification system.
    console.error('Sign-up failed:', error);
    displayErrorMessage(error.message); // Added user-friendly error message display
    } 
    };

Updated Code:
  
    import { sanitize, validateEmail, displayErrorMessage } from './utils';
    const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    try {
      const response = await fetch(`${API_ENDPOINT}/organisations`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: sanitize(organisationName),
        user_name: sanitize(userName),
        email: validateEmail(userEmail) ? userEmail : throw new Error('Invalid email format'),
        password: userPassword.length >= 8 ? userPassword : throw new Error('Password too short'),
      }),
    });

    if (!response.ok) {
      throw new Error(`Sign-up failed: ${response.statusText}`);
    }
    console.log('Sign-up successful');

    const data = await response.json();
    sessionStorage.setItem('authToken', data.token);
    sessionStorage.setItem('userData', JSON.stringify(data.user));
    navigate("/dashboard");
    } catch (error) {
    console.error('Sign-up failed:', error);
    displayErrorMessage(error.message);
    }
    };
## Iterative Development Process

![Untitled Diagram.drawio (1).png](https://www.pupilfirst.school/markdown_attachments/7280/ks9lMWPr7ir9bKRBvTahFw)

## Resolving Merge Conflicts:
If Developer Arun works on feature/enhance-signup and
Developer Adam works on feature/update-profile.
Both modify the same section in Controller.ts.
Developer Arun's Changes
  
    // feature/enhance-signup
    function createUser(req, res) {
    // Original signup logic
    const user = new User({
      name: req.body.name,
      email: req.body.email,
      password: req.body.password,
    });
    }

Developer Adams's Changes

    // feature/update-profile
    function createUser(req, res) {
    // Original signup logic
    // Developer B's enhancement
    const user = new User({
      name: req.body.name,
      email: req.body.email,
      profilePicture: req.body.profilePicture, // Added profile picture
    });
    }
* Fetch Latest Changes:
            
      git fetch origin
* Checkout and Merge Develop Branch:

      git checkout feature/enhance-signup
      git merge origin/develop
* Resolve Conflicts:

      function createUser(req, res) {
      // Original signup logic
      // Combined enhancements
      const user = new User({
       name: req.body.name,
       email: req.body.email,
       password: req.body.password,
       profilePicture: req.body.profilePicture, // Integrated profile picture
       });
      }
* Commit and Push:
   
      git add UserController.ts
      git commit -m "Resolved merge conflicts in createUser function"
      git push origin feature/enhance-signup
## CI/CD Integration:
This helps when we don't want to manually check some boilerplate testing and only check some edge cases.

    # .github/workflows/ci.yml
    name: CI

    on: [push, pull_request]

    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
           - name: Checkout code
           uses: actions/checkout@v2

           - name: Set up Node.js
           uses: actions/setup-node@v2
           with:
             node-version: '14'

           - name: Install dependencies
           run: npm install

           - name: Run tests
           run: npm test

           - name: Lint code
           run: npm run lint

           - name: Build application
           run: npm run build
