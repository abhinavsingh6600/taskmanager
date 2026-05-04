=============================================================================
 __        __         _     ___       _     _ _
 \ \      / /__  _ __| | __/ _ \ _ __| |__ (_) |_
  \ \ /\ / / _ \| '__| |/ / | | | '__| '_ \| | __|
   \ V  V / (_) | |  |   <| |_| | |  | |_) | | |_
    \_/\_/ \___/|_|  |_|\_\\___/|_|  |_.__/|_|\__|

  WorkOrbit - Team Project & Task Management Platform
  Single-File SPA | HTML + CSS + Vanilla JavaScript | LocalStorage
=============================================================================

  Version     : 1.0.0
  Author      : Abhinav Singh 
  Live URL    : https://remarkable-syrniki-bfecb6.netlify.app
  Tech Stack  : HTML5, CSS3, Vanilla JavaScript, Web Storage API
  Type        : Single Page Application (SPA) - No backend required

=============================================================================
  TABLE OF CONTENTS
=============================================================================

   1.  Project Overview
   2.  Live Demo & Deployment
   3.  Key Features
   4.  Roles & Permissions (RBAC)
   5.  Technology Stack
   6.  Installation & Usage
   7.  Demo Credentials (All Accounts)
   8.  Password Rules
   9.  Forgot Password Flow
  10.  Application Views & Navigation
  11.  Data Structure & Persistence
  12.  UI Design System & Color Palette
  13.  Application State (S Object)
  14.  Core JavaScript Functions Reference
  15.  CSS Classes Reference
  16.  LocalStorage Keys
  17.  Resetting the Application
  18.  Known Limitations
  19.  Developer Notes

=============================================================================
  1. PROJECT OVERVIEW
=============================================================================

WorkOrbit is a complete, feature-rich Single Page Application (SPA) for
team collaboration, project management, and task tracking.

It is built entirely using HTML, CSS, and Vanilla JavaScript - no frameworks,
no npm, no build tools, no backend, and no external dependencies required.

All data is persisted client-side using the browser's LocalStorage API,
making it instantly portable and runnable without any server setup.

Core design principles:
  - Role-Based Access Control (RBAC) with Admin and Member tiers
  - Strict authentication with password validation and security questions
  - Interactive Kanban board with per-task progress sliders
  - Fully responsive dark-mode UI using CSS custom properties
  - Zero dependency, zero build step - just one HTML file

=============================================================================
  2. LIVE DEMO & DEPLOYMENT
=============================================================================


  Platform   : Netlify (Static Hosting)
  Deploy Type: Single HTML file (index.html)

  The app is deployed as a static file on Netlify. Since it uses only
  LocalStorage for data, each browser/device maintains its own independent
  data store. There is no shared cloud database.

  To deploy your own copy:
    1. Upload index.html to Netlify (drag & drop in Netlify dashboard)
    2. Or connect a GitHub repo and set build command to "none"
    3. The app is live instantly - no build process needed

=============================================================================
  3. KEY FEATURES
=============================================================================

  AUTHENTICATION SYSTEM
  ----------------------
  - Login with email and password
  - Signup with full name, role selection, email, password, confirm password,
    and a custom security answer (used for password recovery)
  - Strict password validation at signup, reset, and password change
  - "Forgot Password" flow:
      Step 1 - Enter registered email, receive simulated 6-digit code
      Step 2 - Enter 6-digit verification code (auto-advance digit inputs,
                backspace goes to previous box)
      Step 3 - Verify security answer -> set new password
  - Alert notifications (green = success, red = error) auto-dismiss after 4s
  - Sign Out button in sidebar

  DASHBOARD
  ----------
  - 5 live stat cards: Projects, My Tasks, In Progress, Completed, Overdue
  - Color-coded stat values (indigo, cyan, amber, green, red)
  - Red overdue task alert banner (tasks past due date, not yet done)
  - Grid of up to 4 active projects with progress bars and member avatars

  PROJECT MANAGEMENT
  -------------------
  - Create projects: title, description, member selection (pill UI)
  - Edit projects: update name, description, add/remove members
  - Delete project: removes project AND all associated tasks
  - Project cards show: name, description, member avatars (up to 4, +N more),
    task completion progress bar (% Done), and average task progress bar
  - Admin sees ALL projects; Members only see assigned projects
  - Creator is auto-added as owner on every project

  TASK MANAGEMENT & KANBAN BOARD
  --------------------------------
  - Create tasks: title, description, project, assignee, priority, due date
  - Priorities: High (red badge), Medium (amber badge), Low (green badge)
  - Status: To Do | In Progress | Done
  - 3-column Kanban board per project (To Do / In Progress / Done)
  - Interactive range slider (0-100%, step 5) per task on Kanban cards
  - Progress auto-syncs with status:
      0%       -> status set to "To Do"
      1%-99%   -> status set to "In Progress"
      100%     -> status set to "Done"
  - Status dropdown also auto-syncs progress:
      "Done" selected   -> progress forced to 100%
      "To Do" selected  -> progress forced to 0% (only if was 100)
  - Overdue indicator: due date turns red if past today and task not done
  - Edit task: title, description, assignee, priority, due date
  - Delete task: admin only
  - Members without project access see tasks as read-only

  MY TASKS VIEW
  --------------
  - Personal list of ALL tasks assigned to OR created by the logged-in user
  - Full progress slider with 0%, 50%, 100% markers
  - Quick status dropdown per task
  - Clickable link to parent project
  - "Set by assignee" shown if user is not the task assignee and not admin

  TEAM MANAGEMENT  (Admin Only)
  ------------------------------
  - Full table: Member name (with "(you)" tag), Email, Role badge,
    Projects count, Tasks count, Avg Progress bar + percentage
  - Progress bar color: Red (<30%), Amber (30-59%), Indigo (60-99%), Green (100%)
  - Promote member to Admin / Demote admin to Member (toggle button)
  - Remove user: removes from system and all project member lists
  - Cannot remove yourself or change your own role
  - Horizontal scroll on small screens

  SETTINGS PAGE
  --------------
  - Profile: Update full name and email address
  - Change Password: Requires current password + new password (validated)
  - Account Info card: Avatar, name, email, role badge
  - Admin Danger Zone: Manage Team shortcut, Manage Projects shortcut,
    Clear All Tasks (global wipe with confirm dialog)

=============================================================================
  4. ROLES & PERMISSIONS (RBAC)
=============================================================================

  +--------------------------------------------------+--------+--------+
  | Permission                                       | Admin  | Member |
  +--------------------------------------------------+--------+--------+
  | View all projects in system                      |  YES   |   NO   |
  | View only assigned projects                      |  YES   |  YES   |
  | Create new projects                              |  YES   |   NO   |
  | Edit project (name, desc, members)               |  YES   |   NO   |
  | Delete project (removes all tasks too)           |  YES   |   NO   |
  | Create tasks in accessible projects              |  YES   |  YES * |
  | Edit tasks in accessible projects                |  YES   |  YES * |
  | Delete tasks                                     |  YES   |   NO   |
  | Update task status via dropdown                  |  YES   |  YES * |
  | Use task progress slider (Kanban)                |  YES   |  YES * |
  | Use task progress slider (My Tasks)              |  YES   | YES ** |
  | View Team Management page                        |  YES   |   NO   |
  | Promote / Demote user roles                      |  YES   |   NO   |
  | Remove users from system                         |  YES   |   NO   |
  | Access Admin Danger Zone in Settings             |  YES   |   NO   |
  | Clear all tasks globally                         |  YES   |   NO   |
  +--------------------------------------------------+--------+--------+

  *  Member must be assigned to the project.
  ** In My Tasks, progress slider shown only if user is the task assignee.

=============================================================================
  5. TECHNOLOGY STACK
=============================================================================

  Layer             Technology / Detail
  ----------------  ---------------------------------------------------------
  Markup            HTML5 (single monolithic file, no template engine)
  Styling           CSS3
                      - CSS Custom Properties (design tokens via :root)
                      - Flexbox for sidebar, topbar, and card layouts
                      - CSS Grid for stats, project cards, Kanban columns
                      - @keyframes slideIn animation for alert toasts
                      - Custom webkit + moz range input thumb styling
                      - Backdrop-filter blur on modal overlay
  Logic             Vanilla JavaScript (ES6+)
                      - No frameworks (no React, Vue, Angular, jQuery)
                      - No npm packages or external CDN scripts
                      - innerHTML-based SPA render engine
                      - Global state object (S) drives all rendering
  Storage           Web Storage API - window.localStorage
                      Keys: wo_users, wo_projects, wo_tasks
  Architecture      Monolithic SPA - one index.html file
  Hosting           Netlify (static file hosting)
  Browser Support   Chrome, Firefox, Edge, Safari (any modern browser)

=============================================================================
  6. INSTALLATION & USAGE
=============================================================================

  LOCAL USAGE (Offline):
  -----------------------
  Step 1  Create a folder on your computer (e.g. "WorkOrbit")
  Step 2  Inside the folder, create a file named: index.html
  Step 3  Paste the entire WorkOrbit source code into index.html and save
  Step 4  Double-click index.html - it opens directly in your browser
  Step 5  Log in with the demo credentials or create a new account

  No Node.js, no Python server, no npm install, no internet connection
  required after you have the file. It works offline immediately.

  ONLINE DEPLOYMENT (Netlify):
  -----------------------------
  Step 1  Go to https://netlify.com and log in
  Step 2  Drag and drop index.html onto the Netlify dashboard
  Step 3  Your app gets a live URL instantly (e.g. xyz.netlify.app)
  Step 4  Optionally set a custom domain in Netlify settings

  GITHUB + NETLIFY (CI/CD):
  --------------------------
  Step 1  Create a GitHub repo and push index.html to it
  Step 2  Connect the repo to Netlify
  Step 3  Set Build Command: (leave empty)
          Set Publish Directory: . (root)
  Step 4  Every push to main auto-deploys the updated app

=============================================================================
  7. DEMO CREDENTIALS (ALL ACCOUNTS)
=============================================================================

  These are the actual registered accounts in the live demo deployment.

  +--------+------------------------------------+-----------+--------------+
  | Role   | Email                              | Password  | Name         |
  +--------+------------------------------------+-----------+--------------+
  | ADMIN  | farhankhalid17968main@gmail.com    | Farhan@123| Farhan Khalid|
  | MEMBER | farhankhalid17968@gmail.com        | Farhan@123| Member-1     |
  | MEMBER | farhankhalid17968member@gmail.com  | Farhan@123| Member-2     |
  +--------+------------------------------------+-----------+--------------+

  DEFAULT SEED ACCOUNT (auto-created on first load if no data exists):
  +--------+-------------------------+-----------+------------------+
  | Role   | Email                   | Password  | Security Answer  |
  +--------+-------------------------+-----------+------------------+
  | ADMIN  | admin@workorbit.com     | Admin@123 | workorbit        |
  +--------+-------------------------+-----------+------------------+

  NOTE: The seed account is only created if "wo_users" does not exist in
  LocalStorage. On a fresh browser or after clearing storage, this account
  appears automatically.

  You can log in with any of the above credentials immediately,
  or click "Sign Up" to register a new account.

=============================================================================
  8. PASSWORD RULES
=============================================================================

  All passwords across the entire app (signup, change password, reset
  password) must satisfy ALL of the following requirements:

    [1]  Must START with an UPPERCASE letter (A-Z)
    [2]  Must contain at least ONE digit (0-9)
    [3]  Must contain at least ONE special character: ! @ # $ % ^ & *
    [4]  Must be at least 8 characters long in total

  Regular Expression used internally:
    /^[A-Z](?=.*[0-9])(?=.*[!@#$%^&*])(?=.{7,})/

  Valid password examples:
    Admin@123         Farhan@123        WorkOrbit!1
    Secure#99         Project$2024      Build@456

=============================================================================
  9. FORGOT PASSWORD FLOW (3-STEP PROCESS)
=============================================================================

  STEP 1 - Email Entry  (authMode = 'forgot')
  ----------------------------------------------
  - User navigates to "Forgot password?" link on the login screen
  - Enters their registered email address
  - System searches S.users array for a matching email
  - If found: generates a random 6-digit code using Math.random()
  - Code stored in S.resetCode (in-memory only, not persisted)
  - S.authMode set to 'verify', page re-renders to Step 2
  - After 300ms delay, a green alert notification displays the code
    (simulates email delivery - in production, replace with email API)

  STEP 2 - Code Verification  (authMode = 'verify')
  ---------------------------------------------------
  - 6 individual single-digit input boxes displayed in a row
  - Auto-advance: entering a digit moves focus to the next box
  - Backspace: moves focus to the previous box
  - "Verify Code" button joins all 6 inputs and compares to S.resetCode
  - "Resend Code" button calls sendResetCode() again (new code generated)
  - On match: S.authMode = 'reset', renders Step 3

  STEP 3 - Security Answer + New Password  (authMode = 'reset')
  ---------------------------------------------------------------
  - User must correctly answer their security question (set at signup)
  - Comparison is case-insensitive (both sides trimmed + .toLowerCase())
  - User sets a new password + confirms it
  - Both must pass all password rules (see Section 8)
  - On success:
      -> user.password updated in S.users array
      -> save() writes to LocalStorage
      -> S.authMode = 'login', S.resetEmail = '', S.resetCode = ''
      -> Success alert shown, user redirected to login

=============================================================================
  10. APPLICATION VIEWS & NAVIGATION
=============================================================================

  VIEW NAME          S.view value        Access
  -----------------  ------------------  ----------------------------------
  Dashboard          'dashboard'         All logged-in users
  Projects           'projects'          All (admin sees all, member sees own)
  Project Detail     'project-detail'    All (S.subView holds project ID)
  My Tasks           'my-tasks'          All logged-in users
  Team Management    'team'              Admin only
  Settings           'settings'          All logged-in users

  Navigation functions:
    goView(viewName)         Sets S.view, clears S.subView, calls render()
    openProject(projectId)   Sets S.view='project-detail', S.subView=id

  Topbar actions rendered per view:
    'projects'        -> Admin sees "+ New Project" button
    'project-detail'  -> Member (if in project) sees "+ Add Task"
                         Admin sees "+ Add Task", "Edit", "Delete", "<- Back"
    Others            -> No action buttons

=============================================================================
  11. DATA STRUCTURE & PERSISTENCE
=============================================================================

  All data lives in the browser's LocalStorage under 3 keys:

  -- wo_users -----------------------------------------------------------
  Array of user objects. Each object:
  {
    id             : String  - Unique ID (timestamp + random string)
    name           : String  - Full display name
    email          : String  - Login email (lowercase)
    password       : String  - Plaintext password (demo only)
    role           : String  - "admin" | "member"
    color          : String  - Hex color for avatar (cycles through 8 colors)
    securityAnswer : String  - Lowercase security answer for password reset
  }

  -- wo_projects --------------------------------------------------------
  Array of project objects. Each object:
  {
    id         : String    - Unique ID
    name       : String    - Project title
    desc       : String    - Project description
    members    : String[]  - Array of user IDs (creator always first)
    createdBy  : String    - User ID of creator
    createdAt  : String    - ISO timestamp
  }

  -- wo_tasks -----------------------------------------------------------
  Array of task objects. Each object:
  {
    id         : String       - Unique ID
    title      : String       - Task title
    desc       : String       - Task description
    assignee   : String|null  - User ID of assignee (null if unassigned)
    priority   : String       - "high" | "medium" | "low"
    status     : String       - "todo" | "in-progress" | "done"
    progress   : Number       - 0 to 100 (integer, step 5)
    due        : String|null  - Due date string (YYYY-MM-DD) or null
    projectId  : String       - Parent project ID
    createdBy  : String       - User ID of creator
    createdAt  : String       - ISO timestamp
  }

  IMPORTANT: All IDs are stored and compared as Strings. The fixIds()
  function runs on load to normalize any legacy numeric IDs to strings.
  This prevents type-mismatch bugs (e.g. "1" !== 1).

=============================================================================
  12. UI DESIGN SYSTEM & COLOR PALETTE
=============================================================================

  CSS custom properties defined in :root:

  BACKGROUNDS
  -----------
  --bg        #07090F   Main page background (deepest dark)
  --bg2       #0F1320   Sidebar, cards, modals
  --bg3       #161D2E   Input fields, table headers, task mini cards
  --bg4       #1E2740   Progress bar track, Kanban count badge

  BORDERS
  -------
  --border    #1E2740   Subtle borders (cards, sidebar dividers)
  --border2   #2D3A56   Stronger borders (inputs, modals, hover states)

  TEXT
  ----
  --text      #E8EAF6   Primary text (headings, values)
  --text2     #8892B0   Secondary text (labels, descriptions)
  --text3     #4A5568   Tertiary text (placeholders, nav sections, metadata)

  ACCENT COLORS
  -------------
  --indigo    #7C83FD   Primary brand color (active nav, primary buttons,
  --indigo-bg #7C83FD18   progress bars, focus rings)
  --cyan      #22D3EE   Secondary accent (logo "Orbit", gradient ends)
  --cyan-bg   #22D3EE15
  --green     #10B981   Success states, Done status, progress 100%
  --green-bg  #10B98115
  --amber     #F59E0B   In-Progress status, Medium priority, warnings
  --amber-bg  #F59E0B15
  --red       #F43F5E   Error states, High priority, overdue, delete actions
  --red-bg    #F43F5E15
  --purple    #A855F7   Admin role badge
  --purple-bg #A855F715
  --pink      #EC4899   (available, used in avatar color rotation)

  AVATAR COLOR ROTATION (assigned to new users in signup order):
  #7C83FD -> #10B981 -> #F59E0B -> #A855F7 ->
  #F43F5E -> #22D3EE -> #EC4899 -> #6366F1

  PROGRESS COLOR THRESHOLDS (progressColor function):
  0-29%     -> var(--red)
  30-59%    -> var(--amber)
  60-99%    -> var(--indigo)
  100%      -> var(--green)

  TYPOGRAPHY
  ----------
  Font family : "Segoe UI", system-ui, -apple-system, sans-serif
  Base size   : 14px
  Weights used: 400 (normal), 500 (nav), 600 (semi-bold), 700 (bold),
                800 (stat values, project headers), 900 (logo)

=============================================================================
  13. APPLICATION STATE (S OBJECT)
=============================================================================

  The global state object S holds all runtime data:

  S = {
    users          : Array      - All registered users (from LocalStorage)
    projects       : Array      - All projects (from LocalStorage)
    tasks          : Array      - All tasks (from LocalStorage)
    currentUser    : Object|null- Logged-in user object (null = not logged in)
    view           : String     - Current page view name
    subView        : String|null- Project ID when view = 'project-detail'
    authMode       : String     - 'login'|'signup'|'forgot'|'verify'|'reset'
    modal          : String|null- Active modal type or null
    modalData      : Object     - Data passed to active modal
    pendingMembers : Array      - Temp member IDs during project create/edit
    resetEmail     : String     - Email being reset in forgot password flow
    resetCode      : String     - 6-digit verification code (in-memory only)
    _tempCode      : String     - Same as resetCode (debug reference)
  }

  State is mutated directly. After each mutation, render() is called to
  re-render the entire UI via innerHTML replacement on #root.

=============================================================================
  14. CORE JAVASCRIPT FUNCTIONS REFERENCE
=============================================================================

  AUTH FUNCTIONS
  --------------
  handleAuth()              Login or Signup depending on S.authMode
  logout()                  Clears S.currentUser, shows login screen
  startForgot()             Sets S.authMode='forgot', renders forgot step 1
  sendResetCode()           Validates email, generates + stores 6-digit code
  verifyCode()              Joins 6 digit inputs, compares to S.resetCode
  submitReset()             Verifies security answer, sets new password
  codeInputNav(el, i)       Auto-advances focus on digit input (oninput)

  PROJECT FUNCTIONS
  -----------------
  saveProject()             Create or update project (reads modal form)
  deleteProject(id)         Delete project + all its tasks, re-render
  visibleProjects()         Returns projects filtered by user access

  TASK FUNCTIONS
  --------------
  saveTask()                Create or update task (reads modal form)
  deleteTask(id)            Delete task after confirm dialog
  updateTaskStatus(id, s)   Change task status, sync progress, re-render
  updateTaskProgress(id, p) Update progress, sync status, update DOM only
                            (no full re-render - prevents slider jitter)
  canEditTask(task)         Returns true if current user can edit task
  canCreateTaskInProject(p) Returns true if user can add tasks to project

  TEAM / USER FUNCTIONS
  ---------------------
  removeUser(id)            Remove user from system + all project members
  changeRole(id)            Toggle user role between 'admin' and 'member'
  savePassword()            Validate + update password for current user
  saveProfile()             Validate + update name and email for current user

  MEMBER PILL FUNCTIONS (Project Modal)
  --------------------------------------
  addPendingMember()        Add selected user to S.pendingMembers array
  removePendingMember(id)   Remove user from S.pendingMembers
  rerenderPills()           Re-renders only the pills div (partial update)
  pillsHTML()               Returns HTML string for member pills display

  MODAL FUNCTIONS
  ---------------
  openModal(type, data)     Set S.modal + S.modalData, call render()
  closeModal()              Clear S.modal, S.modalData, S.pendingMembers

  RENDER FUNCTIONS
  ----------------
  render()                  Master render - writes entire UI to #root
  renderAuthScreen()        Routes to login, signup, or forgot steps
  renderForgotStep1()       Email entry screen
  renderForgotStep2()       6-digit code verification screen
  renderForgotStep3()       Security answer + new password screen
  renderApp()               Full app shell (sidebar + topbar + content)
  renderTopbar()            Sticky top bar with title and action buttons
  renderView()              Routes to correct page renderer
  renderDashboard()         Stats cards, overdue banner, project grid
  renderProjects()          Project card grid
  renderProjectCard(p)      Single project card HTML
  renderProjectDetail()     Kanban board + project info header
  renderMyTasks()           Personal task list with progress sliders
  renderTeam()              Team management table (admin only)
  renderSettings()          Profile, password, account info, danger zone
  renderModal()             Overlay modal for new/edit project or task

  UTILITY FUNCTIONS
  -----------------
  load(key, default)        Safe LocalStorage JSON parse with fallback
  save()                    Write users, projects, tasks to LocalStorage
  fixIds(arr)               Normalize all IDs to String type on load
  uid()                     Generate unique ID: Date.now() + random base36
  initials(name)            Extract 2-letter avatar initials from name
  tasksDone(pid)            Count done tasks for a project
  tasksTotal(pid)           Count all tasks for a project
  projProgress(pid)         Calculate percentage of tasks done for project
  progressColor(pct)        Return CSS color var based on percentage
  validateEmail(e)          Regex email format validation
  validatePassword(p)       Regex password rules validation
  genCode()                 Generate random 6-digit string for reset flow
  v(id)                     Get value of input/select element by ID
  alert2(msg, color)        Show slide-in toast ('green' or 'red')

  NAVIGATION (exposed on window)
  -------------------------------
  goView(v)                 Set S.view + render
  openProject(id)           Set project-detail view with project ID

=============================================================================
  15. CSS CLASSES REFERENCE
=============================================================================

  LAYOUT
  ------
  .app              Main flex container (sidebar + main)
  .sidebar          Left navigation panel (240px, sticky, 100vh)
  .main             Scrollable content area (flex:1)
  .topbar           Sticky header bar with title and actions
  .page-content     Inner padding wrapper (2rem)

  AUTHENTICATION
  --------------
  .auth-wrap        Full-screen centered flex container with radial glow bg
  .auth-card        Login/signup card (460px, 20px radius, dark bg)
  .auth-logo        Large "WorkOrbit" title in auth card
  .auth-tabs        Login/Signup tab switcher row
  .auth-tab         Individual tab (.active = indigo background)
  .code-inputs      6-box OTP digit input row (flex, centered)
  .code-inputs input Single digit box (46x52px, large font, focus ring)

  NAVIGATION
  ----------
  .sidebar-logo     Logo area at top of sidebar
  .logo-icon        Gradient planet icon (32x32px, indigo->cyan gradient)
  .nav-section      Section label ("MAIN", "MANAGE") in uppercase
  .nav-item         Sidebar link (.active = indigo bg with glow shadow)

  BUTTONS
  -------
  .btn              Base button (inline-flex, 8px radius, transition)
  .btn-primary      Indigo gradient background, white text, glow shadow
  .btn-ghost        Transparent, border, secondary text color
  .btn-danger       Red tinted background, red text, red border
  .btn-success      Green tinted background, green text
  .btn-cyan         Cyan tinted background, cyan text
  .btn-sm           Smaller padding (5px 10px) and font (12px)
  .btn-icon         Square icon button (pencil, trash, close X)

  FORMS
  -----
  .form-group       Label + input wrapper with 1.1rem bottom margin
  .form-row         2-column grid for side-by-side form fields
  .input-with-btn   Flex row combining input and action button

  MODAL
  -----
  .modal-overlay    Fixed full-screen backdrop (88% black, blur 4px)
  .modal            Card container (520px, scrollable, 20px radius)
  .modal-header     Title + close button flex row
  .modal-title      Modal heading text (16px, 700 weight)

  BADGES
  ------
  .badge            Base pill badge (rounded, uppercase, 11px, bold)
  .badge-admin      Purple badge - admin role
  .badge-member     Indigo badge - member role
  .badge-todo       Dark badge - To Do status
  .badge-progress   Amber badge - In Progress status
  .badge-done       Green badge - Done status
  .badge-high       Red badge - High priority
  .badge-medium     Amber badge - Medium priority
  .badge-low        Green badge - Low priority

  STAT CARDS
  ----------
  .stats-grid       Auto-fit grid (min 150px per card, 1rem gap)
  .stat-card        Individual card (::before = 2px top color bar)
  .stat-card.c-indigo   Indigo-to-cyan gradient top bar
  .stat-card.c-amber    Amber top bar
  .stat-card.c-green    Green top bar
  .stat-card.c-red      Red top bar
  .stat-card.c-cyan     Cyan top bar
  .stat-label       Small uppercase label (10px, text3 color)
  .stat-value       Large bold number (28px, 800 weight)

  PROJECT CARDS
  -------------
  .projects-grid    Auto-fill grid (min 300px per card, 1.25rem gap)
  .project-card     Hoverable card (border-left accent, hover lift)
  .progress-bar     Thin track container (4px height, bg4 background)
  .progress-fill    Indigo to cyan gradient fill (animated width 0.4s)

  KANBAN
  ------
  .kanban           3-equal-column grid (repeat(3,1fr), 1rem gap)
  .kanban-col       Single column (bg2, border, 12px radius, min-h 200px)
  .kanban-col-header Column title + count badge (uppercase, 11px)
  .kanban-count     Small count pill (bg4, text3, rounded)
  .task-mini        Mini task card (bg3, border, 9px radius, hover border2)
  .task-mini-title  Task title in Kanban card (13px, 600 weight)
  .task-mini-meta   Priority badge + avatar + due date flex row

  TASK PROGRESS
  -------------
  .progress-slider  Custom range input (4px track, 14px indigo thumb + glow)
  .tp-bar-wrap      80px wide mini bar container (Team table)
  .tp-bar-fill      Colored fill for mini progress bar with transition

  TABLES
  ------
  .task-table       General task table (collapsed, bg2, 12px radius)
  .team-table       Team management table (same base style)
  .scroll-x         Horizontal overflow scroll wrapper

  AVATAR
  ------
  .avatar           32x32px circle with initials (user.color for bg/text)

  MISCELLANEOUS
  -------------
  .alert            Fixed top-right toast (slideIn animation, auto-remove)
  .empty-state      Dashed-border centered placeholder (4rem padding)
  .section-header   Flex row: title left, action button right
  .section-title    Section heading (14px, 700 weight)
  .settings-section Settings block card (bg2, border, 14px radius)
  .danger-zone      Red-border settings card for destructive admin actions
  .tag              Inline colored label pill (same as badge base)
  .member-pills     Flex wrap container for member pill tags
  .member-pill      Member tag (avatar + name + red X remove button)
  .progress-text    Small gray percentage label under progress bar
  .glow-indigo      Utility: indigo box shadow glow effect

=============================================================================
  16. LOCALSTORAGE KEYS
=============================================================================

  Key            Type    Description
  ------------   ------  --------------------------------------------------
  wo_users       JSON    Array of all registered user objects
  wo_projects    JSON    Array of all project objects
  wo_tasks       JSON    Array of all task objects

  To inspect in browser:
    Open DevTools (F12) -> Application tab -> Storage -> Local Storage
    Select your domain to see all three keys and their JSON values

  To manually clear individual keys (DevTools Console):
    localStorage.removeItem('wo_users')
    localStorage.removeItem('wo_projects')
    localStorage.removeItem('wo_tasks')

=============================================================================
  17. RESETTING THE APPLICATION
=============================================================================

  METHOD 1 - Browser DevTools (Recommended)
    Open DevTools -> Application -> Local Storage -> Select your domain
    Click each key (wo_users, wo_projects, wo_tasks) and press Delete
    Refresh the page

  METHOD 2 - DevTools Console (Quick Full Reset)
    localStorage.clear()
    location.reload()

  METHOD 3 - Admin Settings Panel (Tasks Only)
    Log in as Admin -> Settings -> Danger Zone -> "Clear All Tasks"
    This only clears wo_tasks, users and projects remain intact

  METHOD 4 - Browser Clear Cache
    Chrome: Settings -> Privacy -> Clear browsing data
    Check Cookies and site data + Cached images and files -> Clear data
    WARNING: This clears ALL browser data, not just WorkOrbit

  After a full reset, the default seed account is automatically recreated:
    Email: admin@workorbit.com | Password: Admin@123 | Answer: workorbit

=============================================================================
  18. KNOWN LIMITATIONS
=============================================================================

  [1] LocalStorage Only
      Data is browser-specific. Each browser and device has separate data.
      There is NO real-time sync, NO shared database, NO cloud backend.
      Two users opening the same Netlify URL see different data.

  [2] Plaintext Passwords
      Passwords are stored as plaintext strings in LocalStorage.
      This is intentional for a demo/portfolio app. For production use,
      implement server-side authentication with bcrypt/argon2 hashing.

  [3] Simulated Email Delivery
      The password reset code is displayed in an on-screen alert, not
      sent via actual email. To add real email, integrate an API such as
      SendGrid, Resend, or EmailJS.

  [4] No Real-Time Updates Between Tabs
      Since there is no WebSocket or backend, multiple browser tabs do
      not sync automatically. Refreshing the page loads the latest data.

  [5] LocalStorage Quota Limit
      Browsers typically limit LocalStorage to 5-10MB per domain.
      For large teams or very many tasks, this may eventually be exceeded.

  [6] No Drag-and-Drop Kanban
      Tasks move between Kanban columns using the status dropdown, not
      via drag-and-drop. D&D can be added using the HTML5 Drag API.

  [7] No Task Comments or Attachments
      Current version supports: title, description, assignee, priority,
      due date, and progress percentage only. No file uploads or comments.

  [8] No Search or Filter
      There is no global search bar or task filtering by assignee,
      priority, or date range in the current version.

=============================================================================
  19. DEVELOPER NOTES
=============================================================================

  RENDER ENGINE
  The app uses synchronous innerHTML-based rendering. Every call to render()
  re-renders the ENTIRE UI from scratch based on S state. This is simple
  and reliable for a small-to-medium SPA without needing a virtual DOM.

  PARTIAL DOM UPDATES
  updateTaskProgress() is the only function that updates the DOM directly
  without calling render(). This prevents slider focus loss/jitter - if
  render() were called on every oninput event, the slider would reset.

  ID NORMALIZATION
  All IDs throughout the app are treated as Strings. The fixIds() function
  runs at startup on LocalStorage data to coerce any numeric IDs.
  All comparisons use String(x) === String(y) for type safety.

  MODAL CLICK-OUTSIDE-TO-CLOSE
  Modals use onclick="if(event.target===this)closeModal()" on the overlay.
  This only fires if the click target IS the overlay (not a child element),
  implementing click-outside-to-close without any extra event listeners.

  EVENT EXPOSURE
  All event handlers used in innerHTML strings must be on window object.
  The Object.assign(window, {...}) block exposes all necessary functions
  globally so inline onclick attributes in innerHTML strings work correctly.

  MOBILE RESPONSIVENESS
  Stats grid: repeat(auto-fit, minmax(150px, 1fr)) - reflows on small screens
  Projects grid: repeat(auto-fill, minmax(300px, 1fr)) - stacks on mobile
  Team table: wrapped in .scroll-x for horizontal overflow on narrow screens

  SECURITY NOTES (For Production Upgrade)
  - Replace plaintext passwords with server-side bcrypt/argon2 hashing
  - Implement real JWT or session-based authentication
  - Move data to a real database (Supabase, Firebase, PostgreSQL, etc.)
  - Add CSRF protection, rate limiting, and server-side input validation
  - HTTPS is already handled by Netlify for the current deployment

=============================================================================
  Developed and Maintained by Farhan Khalid
  Email   : farhankhalid17968main@gmail.com
  Live App: https://remarkable-syrniki-bfecb6.netlify.app
=============================================================================
