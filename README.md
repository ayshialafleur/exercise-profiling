<img width="1491" height="836" alt="image" src="https://github.com/user-attachments/assets/474f72b9-e36e-4280-ae9c-bb207d57a4bf" />

<img width="1506" height="825" alt="image" src="https://github.com/user-attachments/assets/a65517f0-af7e-410a-b949-70bfacb05660" />

<img width="1916" height="448" alt="image" src="https://github.com/user-attachments/assets/79d1032b-1ea6-41ba-a2b3-a20ab38f9815" />

<img width="1918" height="440" alt="image" src="https://github.com/user-attachments/assets/2acfbca0-b1a4-419c-aeb7-185fed815794" />

<img width="1538" height="393" alt="image" src="https://github.com/user-attachments/assets/e83538a9-63c6-4bc8-ac07-4765c3d88a16" />

<img width="1691" height="410" alt="image" src="https://github.com/user-attachments/assets/f1da9d71-d1a2-4bde-867a-f165b58db68d" />


Conclusion: Based on the refactoring, the application performance improves because expensive operations were moved from Java code into database queries.
For /all-student, the original implementation caused an N+1 query problem: it loaded all students, then queried student_courses for each student one by one. After refactoring, the data is fetched with one JOIN FETCH query, so the number of database queries is greatly reduced.
For /all-student-name, the original method loaded full Student entities and concatenated strings repeatedly using +=. After refactoring, it only queries student names and uses String.join, reducing memory usage and processing time.
For /highest-gpa, the original method loaded all students and searched for the highest GPA in Java. After refactoring, the database returns only the student with the highest GPA using an ordered query, so less data is transferred and processed.

### Reflection
1. JMeter measures external performance (response times and throughput from the user's perspective via protocols), while IntelliJ Profiler measures internal performance (CPU cycles, memory allocation, and method execution time within the JVM).
2. Profiling provides a granular look at the code execution path. It helps us see exactly which methods are "hot spots" (consuming the most CPU) or causing memory bloat, allowing us to pinpoint the exact class or line of code causing delays.
3. Yes. IntelliJ Profiler is highly effective because it integrates directly with the source code, allowing us to jump from a performance bottleneck in a Flame Graph straight to the problematic Java method.
4. The main challenge is Performance Overhead (the tool itself slowing down the app). I overcome this by using Sampling instead of Tracing and conducting tests in a dedicated environment that mimics production.
5. It provides deep visibility into thread states, easy detection of memory leaks, and visual tools like Call Trees that make complex execution paths easy to understand.
6. I treat JMeter as the "What" (the symptom) and the Profiler as the "Why" (the cause). If results differ, I check for external factors like network latency or database locks that JMeter can see but an internal profiler might miss.
7. Strategies include refactoring inefficient algorithms and optimizing database queries. To ensure functionality remains intact, I rely on Automated Unit Tests and Regression Suites to verify that the "fix" hasn't broken existing features.
