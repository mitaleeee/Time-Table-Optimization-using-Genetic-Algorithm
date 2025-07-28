# Timetable Optimization using Genetic Algorithm

This project implements a **Genetic Algorithm (GA)** to automatically generate optimized academic timetables for a university or college. It schedules classes for multiple years, divisions, subjects, professors, classrooms, and timeslots, while considering multiple real-world constraints.

---

## Features

- Multi-year, multi-division support (1st, 2nd, and 3rd year)
- Dynamic subject-professor allocation
- Classroom and time-slot management
- Fitness function based on real academic constraints
- Genetic operations: Selection (with Elitism), Crossover, Mutation
- Avoids:
  - Professor clashes
  - Division clashes
  - Excessive consecutive lectures
  - Uneven daily workloads
  - Mid-day idle hours
- Output:
  - Complete unified timetable
  - Individual division-wise timetable
  - Workload statistics (professors and divisions)

---

## Genetic Algorithm Overview

| Component    | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| Population   | A set of candidate timetables (chromosomes)                                |
| Chromosome   | 3D structure: `[days][slots][classrooms]` representing one full timetable   |
| Fitness      | Penalizes clashes, overloads, idle hours, unbalanced schedules, etc.        |
| Selection    | Tournament selection + Elitism (top 10%)                                    |
| Crossover    | Uniform day-level crossover with repair                                     |
| Mutation     | Slot swaps with conflict checks and repairs                                 |
| Termination  | After a fixed number of generations (default 400)                           |

---

## Technologies Used

- Python 3
- NumPy
- `random` and `collections` modules
- No external libraries required for core logic

---

## Problem Setup

The timetable is to be generated for:

- **3 Academic Years:** 1st, 2nd, 3rd  
- **Each Year Has:** 4 Divisions (A–D), 6 Subjects  
- **Each Subject:** Assigned to one of multiple professors  
- **Classrooms:** 16 total  
- **Timeslots:** 6 slots/day across 5 weekdays  
- **Each Subject:** 4 hours per week per division  

---

## Output Format

The output includes:

- **Complete Weekly Timetable** across all classrooms
- **Division-Wise Timetables** for each class
- **Statistics** for professor workload and subject coverage

---

## Sample Usage

```bash
python timetable_ga.py
````

**Output Sample:**

```
Generating optimal timetable for 3 years with 4 divisions each...
Gen 0: Best -31500.0, Avg -56400.2
Gen 10: Best -22200.0, Avg -45012.3
...

=== COMPLETE TIMETABLE (ALL DIVISIONS) ===
MONDAY
Time     N301                         N302                         ... 
9-10     1st_A-Math (Prof_M1)         ---                          ...
...

=== TIMETABLE FOR 2nd_B ===
TUESDAY
Time     Subject        Professor           Room      
9-10     OOP            Prof_SP             N204      
10-11    DBMS           Prof_GR             N203
...
```

## Constraints Considered

* Each subject is taught exactly 4 times/week
* Professors don’t teach overlapping classes
* No division attends more than one class per slot
* Avoid 3+ consecutive lectures for professors
* Keep idle hours at end of day
* Balance professor workload across weekdays

---

## File Structure

```
📦 timetable-optimizer-ga
├── timetable_ga.py         # Main genetic algorithm implementation
├── README.md               # Project documentation
```

---

## Fitness Function Criteria

| Constraint                      | Penalty Weight |
| ------------------------------- | -------------- |
| Subject hours mismatch          | 10000          |
| Division total hours mismatch   | 10000          |
| Uneven professor daily workload | 1000           |
| 3+ Consecutive professor hours  | 1000           |
| Idle hours in between lectures  | 50             |
