International University of Applied Sciences(iU)
Course Name: DLMDSPWP01 – Programming with Python
Course of Study: Master of Data Management
Author’s name: Cosmas Kyovecho : May, 2026
Matriculation number:    3220417,         Tutor’s name:    Alkubaty, Rami
1. What Was the Assignment About?
Given three CSV data files and asked to write a Python program that:

•	Finds which 4 of 50 mathematical functions best match the 4 training datasets
•	Tests 100 random data points and assigns each one to one of those 4 functions
•	Saves all results into a SQLite database
•	Visualizes all data using interactive charts (Bokeh)
•	Proves the code works using unit tests


1.1 The Three Data Files


File	What It Contains	Think of It As
train.csv	4 functions (y1, y2, y3, y4) with x values from -20 to 20	4 students' messy handwriting samples
ideal.csv	50 clean mathematical functions (y1 to y50)	50 perfect font styles to compare against
test.csv	100 random (x, y) points — unknown origin	100 random written letters to classify


2. The Problem in Simple Terms

3. Available 
•	4 students' handwriting samples — real, slightly messy
•	50 perfect printed font styles — clean and exact
•	100 random handwritten letters — you don't know who wrote them

Job:

1.	Find which font style looks most like each student's handwriting
2.	For each random letter — decide if it was written by student 1, 2, 3, or 4
3.	If the letter looks nothing like any student — ignore it


That is exactly what the Python program does — but with mathematical functions instead of handwriting.

3. Step by Step to Solved It

   Step 1: Load the Data

All three CSV files were loaded into Python using Pandas (like reading an Excel sheet into memory). The data was also saved into a SQLite database using SQLAlchemy — a tool that lets Python talk to databases, similar to how Spring Boot uses JPA in Java.

Database Table	Contents
training_data	x, y1, y2, y3, y4 — the 4 training functions
ideal_functions	x, y1 to y50 — all 50 ideal functions
test_results	x, y, delta_y, ideal_func_no — mapped test points


Step 2: Find the Best 4 Ideal Functions (Least Squares)

For each of the 4 training functions, the program compared it against all 50 ideal functions using a method called Least Squares.

This means:

SSE = sum of (y_training - y_ideal)²   →   Pick the ideal with the SMALLEST SSE

Result we got from the actual data:

Training Function	Best Matching Ideal	SSE (lower = better)	Max Deviation
y1	y13	34.08	0.4992
y2	y24	33.45	0.4990
y3	y36	35.57	0.4989
y4	y40	34.99	0.4998

Step 3: Map Test Data to the Chosen Functions

Each of the 100 test points was checked against the 4 chosen ideal functions (y13, y24, y36, y40). A test point is only assigned if:

|y_test - y_ideal| ≤ max_deviation × √2

This threshold (x √2) gives a little extra tolerance — if a point is within that range, it is a valid match.

Outcome	Count
Test points successfully mapped	34 out of 100
Test points not matched (too far from all 4 functions)	66 out of 100

Step 4: Visualize Everything

Three Bokeh charts were generated

•	Chart 1: Each training function plotted alongside its matched ideal function (2x2 grid)
•	Chart 2: All 100 test points plotted, colored by which ideal function they mapped to
•	Chart 3: Bar chart showing the deviation (delta Y) for each mapped test point

4. What Is NOT Clustering?

This assignment is often confused with clustering. Here is the difference:

Clustering (NOT what we did)	Function Matching (What we DID)
Groups are unknown — algorithm finds them	Functions are already known (the 50 ideal functions)
No reference data provided	We have exact mathematical reference functions
Example: K-Means, DBSCAN	Example: Least Squares curve fitting
Unsupervised learning	Deterministic mathematical matching

5. Python Tools Used and Why

Tool / Library	Purpose	Java Equivalent
Pandas	Read CSV files, handle data tables	List<Map<String, Object>>
SQLAlchemy	Connect to SQLite database, save results	Spring Data JPA / Hibernate
Bokeh	Draw interactive charts in browser	JavaFX Charts
Unittest	Write tests to verify code works correctly	JUnit
math.sqrt()	Calculate square root for threshold	Math.sqrt()

6. Object-Oriented Design (Required by Assignment)

The code was designed using classes and inheritance — a requirement of the assignment. Here is how it maps to Java concepts:

Python Class	Role	Java Equivalent
DatasetBase	Base class with shared loading logic	abstract class DatasetBase
TrainingDataset	Extends DatasetBase, handles train.csv	class TrainingDataset extends DatasetBase
IdealDataset	Extends DatasetBase, handles ideal.csv	class IdealDataset extends DatasetBase
TestDataset	Extends DatasetBase, handles test.csv	class TestDataset extends DatasetBase
DatabaseManager	Handles all database operations	Repository / Service class
FunctionMatcher	Core logic: least squares + mapping	Service class with business logic
Visualizer	Generates all Bokeh charts	Chart rendering class
DatasetLoadError	Custom exception for bad data	class DatasetLoadError extends Exception
MappingError	Custom exception for mapping failures	class MappingError extends Exception


7. Git Commands (Section 1.3 of Assignment)

The assignment also asked to write Git commands for working with the develop branch:

Action	Git Command
Clone the develop branch	git clone --branch develop https://github.com/your-org/your-repo.git
Create a new feature branch	git checkout -b feature/add-new-function
Stage all changes	git add .
Commit with a message	git commit -m "feat: add new function for XYZ"
Push to remote	git push origin feature/add-new-function
Merge into develop	Open Pull Request on GitHub → team reviews → merge

8. Summary in One Sentence

We wrote a Python program that finds which 4 out of 50 mathematical functions best fit the training data using least squares, then checks 100 test points against those 4 functions, saves all results into a SQLite database, and draws interactive charts — all built with proper object-oriented design, custom exceptions, and unit tests

https://colab.research.google.com/drive/1ySoo9evVf7myFUqaGeSGqAk72DbK4doo?usp=sharing
https://colab.research.google.com/drive/1ySoo9evVf7myFUqaGeSGqAk72DbK4doo#scrollTo=HHz3g6uQuI3f
