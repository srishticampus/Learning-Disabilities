# Learning Disabilities Support System (LDSS)

Welcome to the **Learning Disabilities Support System (LDSS)** repository! LDSS is a comprehensive web application designed to bridge the gap between parents, educators, and therapists to support children with learning disabilities.

---

## 📄 Abstract

Children with learning disabilities require tailored guidance, custom educational strategies, continuous progress tracking, and structured therapy plans. The **Learning Disabilities Support System (LDSS)** provides a centralized platform where parents, special educators, and therapists can seamlessly collaborate.

Through LDSS:
- **Parents** can register their children, connect with verified special educators and therapists, enroll children in targeted learning activities, monitor customized learning plans, schedule virtual meetings, track therapy quizzes, and participate in community blogs.
- **Special Educators & Therapists** can evaluate children, design weekly multi-activity learning plans, issue diagnostic or progress quizzes, schedule virtual consultation meetings, and communicate directly with parents via real-time messaging.
- **Administrators** ensure platform safety by reviewing and approving therapist and educator credentials.

---

## 🛠️ Environment Configuration & Setup

### Requirements
- **Node.js**: `v16.x` or higher
- **npm**: `v8.x` or higher
- **MongoDB**: Local MongoDB server running at `mongodb://127.0.0.1:27017` (or MongoDB Atlas)
- **Python** (Optional): `3.8+` with `numpy`, `pydantic` installed (for standalone rating ML script)

---

### Environment Variables (`.env`)

#### 1. Server Environment (`Server/.env`)
Create or edit `Server/.env`:
```env
PORT=4000
SECRET_KEY=your_jwt_secret_key_here
CLIENT_URL=http://localhost:5173
```

#### 2. Client Environment (`Client/.env`)
Create or edit `Client/.env`:
```env
VITE_SERVER_URL=http://localhost:4000
VITE_GEMINI_API_KEY=your_google_gemini_api_key
```

---

### 🚀 Local Installation & Run Guide

#### Step 1: Clone Repository
```bash
git clone <repository-url>
cd Learning-Disabilities-Support
```

#### Step 2: Start Backend Server
```bash
cd Server
npm install
npm start
```
*Server runs at:* `http://localhost:4000` (API Base: `http://localhost:4000/ldss`)

#### Step 3: Start Frontend Client
```bash
cd ../Client
npm install
npm run dev
```
*Client runs at:* `http://localhost:5173`

---

## 👥 MODULES (USERS) & RESPONSIBILITIES

LDSS serves 4 primary user roles (modules):

1. **Admin (`admin`)**: Responsible for platform governance, reviewing and approving professional registrations (Educators and Therapists), and managing system activities.
2. **Parent (`parent`)**: Manages child profiles, browses and sends requests to professionals, tracks assigned weekly learning plans, enrolls children in activities, attempts assigned quizzes, schedules meetings, and engages in chats and blogs.
3. **Educator (`educator`)**: Manages parent connection requests, creates structured academic learning plans, schedules meetings with parents/students, rates progress, and communicates with parents.
4. **Therapist (`theraphist`)**: Manages therapeutic requests, creates therapeutic learning plans, designs diagnostic/progress quizzes for children, tracks quiz attempt results, schedules therapy meetings, and collaborates with linked educators and parents.

---

## ⚙️ COMPLETE LIST OF FUNCTIONS BY MODULE

Below is the exhaustive, complete list of backend functions across all controller modules.

---

### 1. Admin Module (`adminController.js` & `activityController.js`)

| Function Name | Description | Endpoint / Route |
| :--- | :--- | :--- |
| `adminLogin` | Authenticates administrator credentials and returns JWT token | `POST /ldss/admin/login` |
| `educatorRequestAccept` | Approves an educator's registration request | `POST /ldss/admin/educator/accept/:id` |
| `adminDeleteEducator` | Rejects/deletes an educator's registration request | `DELETE /ldss/admin/educator/reject/:id` |
| `theraphistRequestAccept` | Approves a therapist's registration request | `POST /ldss/admin/theraphist/accept/:id` |
| `adminDeleteTheraphist` | Rejects/deletes a therapist's registration request | `DELETE /ldss/admin/theraphist/reject/:id` |
| `editActivity` | Modifies an existing activity details/photo | `PUT /ldss/admin/activities/:id` |
| `getActivityById` | Fetches details of a specific activity | `GET /ldss/admin/activity/:id` |

---

### 2. Parent Module (`parentController.js`, `childController.js`, `quizController.js`)

| Function Name | Description | Endpoint / Route |
| :--- | :--- | :--- |
| `parentRegister` | Registers a new parent with profile picture | `POST /ldss/parent/registration` |
| `parentLogin` | Authenticates parent login and issues JWT token | `POST /ldss/parent/login` |
| `parentForgotPassword` | Verifies parent email for password reset | `POST /ldss/parent/forgotpassword` |
| `parentResetPassword` | Updates parent password upon reset | `POST /ldss/parent/resetpassword/:email` |
| `getParentById` | Retrieves parent profile details by ID | `GET /ldss/parent/getparent/:id` |
| `getAllParents` | Retrieves list of all registered parents | `GET /ldss/parent/getallparents` |
| `editParentById` | Updates parent profile info and avatar | `POST /ldss/parent/updateparent/:id` |
| `getAcceptedEducator` | Fetches educators who accepted parent's requests | `GET /ldss/parent/getacceptededucator/:id` |
| `getAcceptedTherapists` | Fetches therapists who accepted parent's requests | `GET /ldss/parent/getacceptedtherapist/:id` |
| `addChildByParent` | Adds a new child profile under the parent | `POST /ldss/parent/addchild/:id` |
| `editChildByParent` | Updates child profile details | `POST /ldss/parent/updatechild/:id/:childId` |
| `getOneChildDetail` | Gets details of a specific child | `GET /ldss/parent/getchild/:id/:childId` |
| `getallChildDetails` | Retrieves all children profiles in the system | `GET /ldss/parent/getallchild` |
| `getAllChildOfParent` | Retrieves all children profiles belonging to a specific parent | `GET /ldss/parent/getallchildofparent/:id` |
| `deleteChildByParent` | Removes a child profile | `DELETE /ldss/parent/deletechild/:id/:childId` |
| `updateLearningPlanByParent` | Marks educator learning plan as completed | `PUT /ldss/parent/updatelearningplan/:childId/:educatorId` |
| `updateLearningPlanByParentTherapist` | Marks therapist learning plan as completed | `PUT /ldss/parent/updatelearningplantherapist/:childId/:therapistId` |
| `markActivityCompleted` | Marks a specific weekly activity in a learning plan as completed | `PUT /ldss/parent/completeactivity/:planId/:weekIndex/:activityIndex` |
| `getParentQuizzesForChild` | Fetches quizzes assigned to child with attempt status | `GET /ldss/parent/quizzes/child/:childId` |
| `getParentQuizAttemptDetails` | Fetches detailed report of a completed quiz attempt | `GET /ldss/parent/quiz/attempt/:attemptId` |
| `submitQuizAttempt` | Submits answers for a quiz attempt and calculates score | `POST /ldss/parent/quiz/submit/:quizId` |

---

### 3. Educator Module (`educatorController.js`, `learningController.js`, `meetingController.js`)

| Function Name | Description | Endpoint / Route |
| :--- | :--- | :--- |
| `educatorRegister` | Registers a new educator profile | `POST /ldss/educator/registration` |
| `educatorLogin` | Authenticates educator (requires Admin approval) | `POST /ldss/educator/login` |
| `educatorForgotPassword` | Initiates password reset for educator | `POST /ldss/educator/forgotpassword` |
| `educatorResetPassword` | Resets educator password | `POST /ldss/educator/resetpassword/:email` |
| `getEducatorById` | Retrieves educator profile by ID | `GET /ldss/educator/geteducator/:id` |
| `editEducatorById` | Updates educator basic profile details | `POST /ldss/educator/updateeducator/:id` |
| `addEducatorPersonal` | Adds qualifications, experience, availability, & certification | `POST /ldss/educator/addpersonal/:id` |
| `getAllEducators` | Fetches all registered educators | `GET /ldss/educator/getalleducators` |
| `getEducatorRequest` | Lists pending requests sent by parents | `GET /ldss/educator/parentsrequest/:id` |
| `educatorAcceptsParentRequest` | Accepts parent request & initializes chat conversation | `PUT /ldss/educator/acceptsrequest/:id` |
| `educatorRejectParentRequest` | Rejects/deletes parent request | `DELETE /ldss/educator/rejectparent/:id` |
| `educatorViewRequestById` | Views specific parent request details | `GET /ldss/educator/viewrequestedparent/:id` |
| `viewAllApprovedParentsByEducator` | Fetches all parents accepted by educator | `GET /ldss/educator/getapprovedparents/:id` |
| `viewAllChildsOfAllApprovedParents` | Fetches all children of accepted parents | `GET /ldss/educator/getchildrenofallapprovedparents/:id` |
| `addLearningPlan` | Creates a new weekly educational plan for a child | `POST /ldss/educator/addlearning` |
| `getLearningPlanOfSingleStudent` | Retrieves educator learning plan for a child | `GET /ldss/educator/getstudentplan/:educatorId/:childId` |
| `editLearningPlanByEducator` | Updates an existing educational learning plan | `PUT /ldss/educator/updateplan/:educatorId/:childId` |
| `deleteLearningPlanByEducator` | Deletes an educational learning plan | `DELETE /ldss/educator/deleteplan/:id` |
| `createMeeting` | Schedules a meeting with parent for a child | `POST /ldss/educator/addmeeting/:id/:childId` |
| `viewChildsMeeting` | Views scheduled meetings for a specific child | `GET /ldss/educator/viewmeeting/:id/:childId` |
| `viewAllmeetingsOfEducator` | Retrieves all meetings created by educator | `GET /ldss/educator/viewmeeting/:id` |

---

### 4. Therapist Module (`theraphistController.js`, `quizController.js`, `learningController.js`)

| Function Name | Description | Endpoint / Route |
| :--- | :--- | :--- |
| `theraphistRegister` | Registers a new therapist profile | `POST /ldss/theraphist/registration` |
| `theraphistLogin` | Authenticates therapist (requires Admin approval) | `POST /ldss/theraphist/login` |
| `theraphistForgotPassword` | Initiates password reset for therapist | `POST /ldss/theraphist/forgotpassword` |
| `theraphistResetPassword` | Resets therapist password | `POST /ldss/theraphist/resetpassword/:email` |
| `getTheraphistById` | Retrieves therapist profile by ID | `GET /ldss/theraphist/gettheraphist/:id` |
| `editTheraphistById` | Updates basic therapist profile details | `POST /ldss/theraphist/updatetheraphist/:id` |
| `getAllTheraphists` | Fetches all registered therapists | `GET /ldss/theraphist/getalltheraphist` |
| `addTheraphistPersonal` | Adds qualifications, experience, specialities & certificate | `POST /ldss/theraphist/addpersonal/:id` |
| `getTherapistRequests` | Fetches incoming parent requests | `GET /ldss/theraphist/parentsrequest/:id` |
| `acceptParentRequest` | Accepts parent request & initializes chat conversation | `POST /ldss/theraphist/acceptrequest/:id` |
| `rejectParentRequest` | Rejects/deletes parent request | `POST /ldss/theraphist/rejectrequest/:id` |
| `viewRequestById` | Views specific request details | `GET /ldss/theraphist/viewrequestedparent/:id` |
| `viewAllApprovedParentsByTherapist` | Fetches all parents accepted by therapist | `GET /ldss/theraphist/getapprovedparents/:id` |
| `viewAllChildsOfApprovedParents` | Fetches all children belonging to accepted parents | `GET /ldss/theraphist/getchildrenofallapprovedparents/:id` |
| `getLinkedEducators` | Finds educators working with the same children/parents | `GET /ldss/therapist/linkededucators/:id` (Controller helper) |
| `addLearningPlanTherapist` | Creates a therapeutic learning plan for a child | `POST /ldss/theraphist/addlearning` |
| `getLearningPlanOfSingleTherapist` | Retrieves therapist learning plan for a child | `GET /ldss/theraphist/getstudentplan/:therapistId/:childId` |
| `editLearningPlanByTherapist` | Updates a therapeutic learning plan | `PUT /ldss/theraphist/updateplan/:therapistId/:childId` |
| `deleteLearningTherapist` | Deletes a therapeutic learning plan | `DELETE /ldss/theraphist/deleteplan/:id` |
| `createQuiz` | Creates a customized diagnostic/progress quiz for a child | `POST /ldss/therapist/quizzes` |
| `getTherapistQuizzesForChild` | Fetches quizzes created by therapist for a child | `GET /ldss/therapist/quizzes/child/:childId` |
| `getTherapistQuizAttemptsForChild` | Retrieves child's attempt history for therapist quizzes | `GET /ldss/therapist/quizzes/attempts/:childId` |
| `getTherapistQuizAttemptDetails` | Fetches details of a specific quiz attempt | `GET /ldss/therapist/quiz/attempt/:attemptId` |
| `deleteQuiz` | Deletes a quiz and all associated attempts | `DELETE /ldss/therapist/quiz/:quizId` |
| `viewAllmeetingsOfTherapist` | Views all meetings scheduled by therapist | `GET /ldss/therapist/viewmeeting/:id` |

---

### 5. Shared & System Features

#### A. Activity Management (`activityController.js`)
- `addActivity`: Creates a new general activity with thumbnail (`POST /ldss/addactivity`).
- `getAllActivities`: Fetches all platform activities (`GET /ldss/activity/getallactivities`).
- `getEnrolledActivitiesByParent`: Fetches activities enrolled by parent (`GET /ldss/activity/parent/:parentId`).
- `enrollActivity`: Enrolls a parent in an activity (`POST /ldss/activity/enroll/:activityId/:parentId`).
- `markActivityComplete`: Marks an enrolled activity as completed (`PATCH /ldss/activity/complete/:activityId`).
- `deleteActivity`: Removes an activity and deletes image (`DELETE /ldss/activity/delete/:id`).

#### B. Connection Requests (`requestController.js`)
- `sendRequest`: Sends connection request from parent to educator/therapist (`POST /ldss/request/sendrequest`).
- `fetchAll`: Retrieves all system requests (`GET /ldss/request/fetchall`).

#### C. Real-Time Chat System (`chatController.js`)
- `createConversation`: Starts chat between parent and educator/therapist (`POST /ldss/conversations`).
- `getUserConversations`: Fetches user's active conversations (`GET /ldss/conversations/user/:userId`).
- `getConversation`: Retrieves single conversation and message thread (`GET /ldss/conversations/:id`).
- `addMessage`: Appends message to chat thread (`POST /ldss/conversations/:id/messages`).
- `markAsRead`: Marks unread messages in conversation as read (`PATCH /ldss/conversations/:id/read`).

#### D. Rating & Review System (`ratingController.js` & `rating_ml.py`)
- `addOrUpdateRating`: Submits or updates star rating for a professional (`POST /ldss/addrating`).
- `getSpecificRating`: Fetches rating submitted by parent for a professional (`GET /ldss/specific`).
- `getAverageRatingForProfessional`: Computes average rating and count (`GET /ldss/average/:professionalType/:professionalId`).
- `getRatingsByProfessionalType`: Lists ratings categorized by educator/therapist (`GET /ldss/ratings/:professionalType`).
- `get_professional_rating` (*Python script*): ML rating algorithm applying consistency weighting for educators and time-decay weighting for therapists.

#### E. Community Blog System (`blogController.js`)
- `addBlog`: Creates a new community blog post (`POST /ldss/blog/add`).
- `editBlog`: Updates existing blog title, content, or cover image (`PUT /ldss/blog/edit/:id`).
- `fetchAllBlogs`: Retrieves paginated list of blog posts (`GET /ldss/blog/all`).
- `fetchBlogById`: Retrieves single blog post by ID (`GET /ldss/blog/:id`).
- `addLike`: Likes a blog post (`POST /ldss/blog/like/:id`).
- `removeLike`: Unlikes a blog post (`DELETE /ldss/blog/unlike/:id`).
- `removeBlog`: Deletes a blog post (`DELETE /ldss/blog/delete/:id`).

---

## 📌 Summary Table of Key API Endpoints

| Category | Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- | :--- |
| **Auth** | `POST` | `/ldss/admin/login` | Admin login | No |
| **Auth** | `POST` | `/ldss/parent/registration` | Register parent | No |
| **Auth** | `POST` | `/ldss/educator/registration` | Register educator | No |
| **Auth** | `POST` | `/ldss/theraphist/registration` | Register therapist | No |
| **Child** | `POST` | `/ldss/parent/addchild/:id` | Add child | Yes |
| **Request**| `POST` | `/ldss/request/sendrequest` | Request professional | Yes |
| **Plan** | `POST` | `/ldss/educator/addlearning` | Create educator plan | Yes |
| **Quiz** | `POST` | `/ldss/therapist/quizzes` | Create therapist quiz | Yes |
| **Quiz** | `POST` | `/ldss/parent/quiz/submit/:quizId` | Submit quiz attempt | Yes |
| **Chat** | `POST` | `/ldss/conversations/:id/messages`| Send chat message | Yes |

---
