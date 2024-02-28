### Normal Forms in Database Design

Normal forms in database design are a set of guidelines to reduce data redundancy and improve data integrity. They are particularly important in relational database design. Let's go through the first three normal forms (1NF, 2NF, 3NF) and the Boyce-Codd Normal Form (BCNF), along with examples:

#### 1. First Normal Form (1NF)
**Definition:** A table is in 1NF if it only contains atomic (indivisible) values and each record is unique.
- **Atomic values:** No multi-valued attributes. For example, a column cannot hold a list or a set of values.
- **Unique records:** No duplicate rows in the table.

**Example:**
Consider a table with student data.

| StudentID | Name  | Subjects        |
|-----------|-------|-----------------|
| 1         | Alice | Math, Science   |
| 2         | Bob   | History         |
| 3         | Alice | Math, English   |

This table is not in 1NF because the 'Subjects' column contains multiple values. To convert it into 1NF, we can modify it as follows:

| StudentID | Name  | Subject  |
|-----------|-------|----------|
| 1         | Alice | Math     |
| 1         | Alice | Science  |
| 2         | Bob   | History  |
| 3         | Alice | Math     |
| 3         | Alice | English  |

#### 2. Second Normal Form (2NF)
**Definition:** A table is in 2NF if it is in 1NF and all non-key attributes are fully functionally dependent on the primary key.
- **Fully functionally dependent:** A column value is fully dependent on the entire primary key, not just part of it.

**Example:**
Continuing with our student table, suppose we have:

| StudentID | Subject  | Teacher       |
|-----------|----------|---------------|
| 1         | Math     | Mr. Smith     |
| 1         | Science  | Mrs. Johnson  |
| 2         | History  | Mr. Allen     |

Assuming (StudentID, Subject) is the composite primary key, this table is in 1NF but not in 2NF because 'Teacher' is dependent on 'Subject', not the entire key. We can split it into two tables to achieve 2NF:

- Student_Subjects table:

  | StudentID | Subject  |
  |-----------|----------|
  | 1         | Math     |
  | 1         | Science  |
  | 2         | History  |

- Subjects_Teachers table:

  | Subject  | Teacher       |
  |----------|---------------|
  | Math     | Mr. Smith     |
  | Science  | Mrs. Johnson  |
  | History  | Mr. Allen     |

#### 3. Third Normal Form (3NF)
**Definition:** A table is in 3NF if it is in 2NF and all the attributes are functionally dependent only on the primary key.
- **No transitive dependencies:** Non-primary key columns should not depend on other non-primary key columns.

**Example:**
Consider a table:

| StudentID | Course     | Instructor  | InstructorPhone |
|-----------|------------|-------------|-----------------|
| 1         | Biology    | Dr. Lee     | 123-456-7890    |
| 2         | Chemistry  | Dr. Clark   | 987-654-3210    |

Even if this table is in 2NF, it's not in 3NF because 'InstructorPhone' is dependent on 'Instructor', not directly on 'StudentID' or 'Course'. To convert it to 3NF, we split the table:

- Student_Courses table:

  | StudentID | Course    |
  |-----------|-----------|
  | 1         | Biology   |
  | 2         | Chemistry |

- Instructors table:

  | Instructor | InstructorPhone |
  |------------|-----------------|
  | Dr. Lee    | 123-456-7890    |
  | Dr. Clark  | 987-654-3210    |

#### Boyce-Codd Normal Form (BCNF)
**Definition:** A table is in BCNF if it is in 3NF and for every functional dependency (X → Y), X is a super key.
- **Super key:** A set of attributes which uniquely identifies a row.

**Example:**
Consider a table:

| Building | RoomNo | Capacity |
|----------|--------|----------|
| A        | 101    | 40       |
| B        | 102    | 30       |
| A        | 103    | 40       |

Here, (Building, RoomNo) is the primary key. However, Capacity is functionally dependent on RoomNo alone, which is not a super key. To bring this table into BCNF, we can split it into two tables:

- Buildings_Rooms table:

  | Building | RoomNo |
  |----------|--------|
  | A        | 101    |
  | B        | 102    |
  | A        | 103    |

- Rooms_Capacity table:

  | RoomNo | Capacity |
  |--------|----------|
  | 101    | 40       |
  | 102    | 30       |
  | 103    | 40       |

These normal forms help in structuring databases in a way that reduces redundancy and improves data integrity. Each subsequent normal form addresses additional types of data anomalies.
