# UniSched — University Timetable Planner

**Turn course constraints into a weekly teaching plan.**

UniSched helps university coordinators generate course timetables around instructor availability, classroom capacity and course requirements. Built with Django, it brings automatic scheduling, Excel-based enrollment and role-specific schedule views into one workflow, with manual adjustments available through the admin panel.

---

## 🔧 **Core Features**

- **Automatic Course Scheduling Algorithm**  
  - Assigns courses to available time slots without overlapping instructors or students  
  - Takes classroom availability, course types (theory/lab), and capacity constraints into account  
  - Supports fixed day/hour enforcement for certain courses if needed

- **Excel-Based Student Course Enrollment**  
  - Upload Excel files to batch assign students to courses  
  - Automatically creates new student/instructor accounts if not found in the system  
  - Supports multi-department course enrollments

- **Role-Based User System**  
  - **Coordinator**: Generates timetables, manages departments, and performs admin tasks  
  - **Instructor**: Views assigned courses and schedules  
  - **Student**: Views personal schedule and registered courses

- **Manual Adjustments via Admin Panel**  
  - Timetables can be edited manually after automatic generation  
  - Schedule is shown in visual grid format (similar to a classic weekly calendar)

---

## 🧠 **Scheduling Logic Overview**

The scheduling algorithm works by:

1. **Analyzing All Constraints**  
   - Instructor availability (unavailable days/hours)  
   - Room types and capacities  
   - Course hour requirements (e.g., 2-hour per week)  
   - Avoiding overlaps between departments and shared courses  

2. **Greedy + Backtracking Assignment**  
   - Greedy logic tries best-fit placements  
   - If unsatisfiable, the algorithm backtracks and tries alternatives  
   - Ensures balanced distribution of courses across the week

3. **Final Output**  
   - Saves the generated timetable to the database  
   - Displays it in a table for each class and department  
   - Excel export available

---

## 📂 **Project Modules**

- `models.py` – Database models for users, courses, departments, etc.  
- `admin.py` – Custom admin views, Excel upload logic, and UI integration  
- `scheduler.py` – Main algorithm logic for course placement
