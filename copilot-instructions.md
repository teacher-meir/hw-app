# Homework App - Project Documentation

## Project Overview
A web application for managing homework and project submissions, built with **Next.js** and **Firebase**. Uses Material-UI components with Tailwind CSS for styling and deploys to GitHub Pages via GitHub Actions.

## Tech Stack
- **Frontend/Backend**: Next.js (App Router)
- **UI Framework**: Material-UI (MUI)
- **Styling**: Tailwind CSS
- **Database**: Firebase (Firestore)
- **Authentication**: Firebase Authentication
- **File Storage**: Firebase Cloud Storage
- **Hosting**: GitHub Pages
- **CI/CD**: GitHub Actions
- **Package Manager**: npm

## Project Features

### 1. Authentication (Login/Signup)
- User registration with email and password
- Login functionality
- Password reset capability
- Firebase Authentication integration

### 2. Homework/Project Upload
- Upload files (PDFs, documents, images, etc.)
- Set assignment details (title, description, github link, due date)
- Categorize submissions (homework, project, etc.)
- Store in Firebase Cloud Storage

### 3. Submission Display
- View all submissions
- Filter by category, date, or status
- View submission details
- Download submitted files
- Track submission status (pending, submitted, graded)

## Project Structure
```
hw-app/
├── app/
│   ├── layout.tsx          # Root layout
│   ├── page.tsx            # Home page
│   ├── globals.css         # Global styles with Tailwind
│   ├── dashboard/
│   │   └── page.tsx
│   ├── upload/
│   │   └── page.tsx
│   ├── submissions/
│   │   └── page.tsx
│   └── auth/
│       ├── login/page.tsx
│       ├── signup/page.tsx
│       └── password-reset/page.tsx
├── components/
│   ├── Auth/
│   │   ├── Login.tsx
│   │   ├── SignUp.tsx
│   │   └── PasswordReset.tsx
│   ├── Dashboard/
│   │   ├── Dashboard.tsx
│   │   └── Sidebar.tsx
│   ├── Upload/
│   │   ├── FileUpload.tsx
│   │   └── AssignmentForm.tsx
│   ├── Submissions/
│   │   ├── SubmissionList.tsx
│   │   ├── SubmissionCard.tsx
│   │   └── SubmissionDetail.tsx
│   └── Common/
│       ├── Header.tsx
│       ├── Footer.tsx
│       └── NavBar.tsx
├── services/
│   ├── authService.ts
│   ├── firestoreService.ts
│   └── storageService.ts
├── hooks/
│   ├── useAuth.ts
│   └── useSubmissions.ts
├── utils/
│   ├── constants.ts
│   ├── validation.ts
│   └── helpers.ts
├── public/
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions workflow
├── next.config.ts
├── tsconfig.json
├── package.json
├── tailwind.config.ts      # Tailwind configuration
├── postcss.config.mjs      # PostCSS configuration
├── .env.example
├── .gitignore
└── PROJECT.md
```

## Firebase Setup
1. Create Firebase project at firebase.google.com
2. Enable Firestore Database (test mode for development)
3. Enable Authentication (Email/Password)
4. Enable Storage for file uploads
5. Add Firebase config to `.env` file

## Environment Variables
```
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

## Installation & Setup
1. Clone the repository
2. Install dependencies: `npm install`
3. Configure Firebase environment variables in `.env.local`
4. Start dev server: `npm run dev`
5. Build for production: `npm run build`
6. Test the build locally: `npm run start`

## Key Dependencies
- next: React framework with server-side rendering
- react & react-dom: React library
- firebase: Backend services
- @mui/material: Material Design components
- @mui/icons-material: MUI icons
- @emotion/react & @emotion/styled: MUI styling dependencies
- tailwindcss: Utility-first CSS framework
- typescript: Type safety

## Development Workflow
1. Create page routes in `app/` directory using Next.js App Router
2. Create reusable components in `components/` folder
3. Create services for Firebase operations in `services/` folder
4. Use custom hooks in `hooks/` folder for state management and logic
5. Use MUI components with Tailwind CSS for styling
6. Test locally with `npm run dev`
7. Push to GitHub and GitHub Actions automatically deploys to GitHub Pages

## Unit Testing

### Testing Framework & Tools
- **Jest**: Unit and integration testing framework
- **React Testing Library**: Component testing utilities
- **@testing-library/jest-dom**: Custom Jest matchers
- **@testing-library/user-event**: User interaction simulation

### Installation
```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom @testing-library/user-event ts-node jest-environment-jsdom
```

### Test File Organization
- Test files placed alongside source files with `.test.ts` or `.test.tsx` suffix
- Example: `components/Auth/Login.tsx` → `components/Auth/Login.test.tsx`
- Shared test utilities in `__tests__/` or `utils/testHelpers.ts`

### Testing Guidelines

#### 1. Component Tests
- Test user interactions and rendered output
- Use `render()` from React Testing Library
- Query by role, label, or accessible text
- Example:
```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Login from './Login';

describe('Login Component', () => {
  it('should submit form with valid credentials', async () => {
    render(<Login />);
    const emailInput = screen.getByLabelText(/email/i);
    await userEvent.type(emailInput, 'test@example.com');
    const submitButton = screen.getByRole('button', { name: /login/i });
    await userEvent.click(submitButton);
    // Assert expected behavior
  });
});
```

#### 2. Service Tests
- Mock Firebase calls using Jest mocks
- Test error handling and edge cases
- Test data transformation logic
- Example:
```typescript
import { authService } from './authService';
jest.mock('firebase/auth');

describe('authService', () => {
  it('should handle login errors gracefully', async () => {
    // Mock Firebase error
    // Test error handling
  });
});
```

#### 3. Hook Tests
- Use `renderHook()` from React Testing Library
- Test state changes and side effects
- Wrap hooks with proper providers if needed
- Example:
```typescript
import { renderHook, act } from '@testing-library/react';
import { useAuth } from './useAuth';

describe('useAuth Hook', () => {
  it('should update user on login', async () => {
    const { result } = renderHook(() => useAuth());
    act(() => {
      result.current.login('test@example.com', 'password');
    });
    expect(result.current.user).toBeDefined();
  });
});
```

#### 4. Utility Functions Tests
- Test pure functions with various inputs
- Include edge cases and error scenarios
- Example:
```typescript
import { validateEmail } from './validation';

describe('validateEmail', () => {
  it('should validate correct email formats', () => {
    expect(validateEmail('test@example.com')).toBe(true);
  });
  
  it('should reject invalid email formats', () => {
    expect(validateEmail('invalid-email')).toBe(false);
  });
});
```

### Running Tests
```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage report
npm test -- --coverage

# Run specific test file
npm test Login.test.tsx
```

### Coverage Goals
- Aim for at least 80% code coverage
- Critical paths (auth, data submission) should have 90%+ coverage
- Focus on testing behavior, not implementation details

### Best Practices
- Write tests as you write code, not after
- Keep tests focused and isolated
- Use descriptive test names
- Mock external dependencies (Firebase, APIs)
- Avoid testing implementation details; test user behavior
- Use `beforeEach` for setup, `afterEach` for cleanup
- Group related tests with `describe` blocks
- Use `screen` queries over container queries when possible

## GitHub Pages Deployment with Actions
1. Ensure `basePath` and `assetPrefix` are configured in `next.config.ts` for GitHub Pages
2. GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
   - Builds the Next.js project
   - Exports static files
   - Deploys to `gh-pages` branch
3. Enable GitHub Pages in repository settings to use `gh-pages` branch
4. Deployment triggered on push to main branch

## Notes
- Use environment variables (`.env.local`) for sensitive Firebase config
- MUI components can be styled with Tailwind CSS classes
- Next.js App Router provides file-based routing
- Use TypeScript for type safety in components and services
- Implement proper error handling for file uploads
- Add loading states and user feedback
- Validate file types and sizes before upload
- Implement proper access control in Firestore rules
- Use `'use client'` directive for client-side components in App Router
