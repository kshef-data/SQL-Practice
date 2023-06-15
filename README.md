# SQL-Practice
https://www.sql-practice.com/ 
EASY Daily practice

#Show first name, last name, and gender of patients who's gender is 'M'

SELECT first_name, last_name, gender
FROM patients
WHERE gender = 'M';

#Show first name and last name of patients who does not have allergies. (null)

SELECT first_name, last_name
FROM patients
WHERE allergies is NULL;

#Show first name of patients that start with the letter 'C'

SELECT first_name
FROM patients
WHERE first_name like 'C%';

#Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

SELECT first_name, last_name
FROM patients
WHERE weight BETWEEN 100 and 120;

#Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

UPDATE patients
set allergies = 'NKA'
WHERE allergies is NULL;

##In this query, the UPDATE statement is used to modify the data in the "patients" table. The SET clause sets the value of the "allergies" column to 'NKA'. The WHERE clause filters the rows and only updates the ones where the "allergies" column is null.

#Show first name and last name concatinated into one column to show their full name.
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM patients

#Show first name, last name, and the full province name of each patient. Example: 'Ontario' instead of 'ON'

SELECT first_name, last_name, province_name
FROM patients
JOIN province_names ON province_names.province_id = patients.province_id;

#Show how many patients have a birth_date with 2010 as the birth year.

select COUNT(*) AS total_patients
FROM patients
WHERE YEAR (birth_date) = 2010;

#Show the first_name, last_name, and height of the patient with the greatest height.

select first_name, last_name, height
FROM patients
WHERE height = (SELECT MAX(height) FROM patients);

#Show all columns for patients who have one of the following patient_ids: 1,45,534,879,1000

SELECT *
FROM patients
WHERE patient_id IN (1, 45, 534, 879, 1000);

#Show the total number of admissions

SELECT COUNT(*) AS total_admissions
FROM admissions

#Show all the columns from admissions where the patient was admitted and discharged on the same day.

SELECT *
FROM admissions
WHERE admission_date = discharge_date;

#Show the patient id and the total number of admissions for patient_id 579.

SELECT patient_id, 
COUNT(*) AS total_admissions
FROM admissions
WHERE patient_id = 579;

#Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

SELECT DISTINCT(city) AS unique_cities
FROM patients
WHERE province_id = 'NS';

#Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70

SELECT first_name, last_name, birth_date
FROM patients
WHERE height > 160
AND weight > 70;

#Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null

SELECT first_name, last_name, allergies
FROM patients
WHERE city = 'Hamilton'
AND allergies IS NOT NULL;

#Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the result order in ascending by city.

SELECT DISTINCT city
FROM patients
WHERE city LIKE 'A%' OR city LIKE 'E%' OR city LIKE 'I%' OR city LIKE 'O%' OR city LIKE 'U%'
ORDER BY city ASC;

MEDIUM Daily practice
#Show unique birth years from patients and order them by ascending.

SELECT DISTINCT YEAR(birth_date) AS birth_year
FROM patients
ORDER BY birth_date ASC;

#Show unique first names from the patients table which only occurs once in the list. For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

SELECT first_name
FROM patients
group by first_name
HAVING COUNT(first_name) = 1;

#Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

SELECT patient_id, first_name
FROM patients
WHERE first_name LIKE 'S____%S';

#Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'. Primary diagnosis is stored in the admissions table.

SELECT patients.patient_id, first_name, last_name
FROM patients
JOIN admissions ON admissions.patient_id = patients.patient_id
WHERE diagnosis = 'Dementia';

#Display every patient's first_name. Order the list by the length of each name and then by alphbetically
FROM patients

SELECT first_name
FROM patients
ORDER BY LEN(first_name), first_name;

#Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.

SELECT
(SELECT COUNT(*) FROM patients WHERE gender = 'M') AS male_count,
(SELECT COUNT(*) FROM patients WHERE gender = 'F') AS female_count; 

#Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.

SELECT first_name, last_name, allergies
FROM patients
WHERE allergies IN ('Penicillin', 'Morphine')
ORDER BY allergies ASC, first_name ASC, last_name ASC;

#Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

SELECT patient_id, diagnosis
FROM admissions
group by patient_id, diagnosis
HAVING COUNT(*) > 1;

#Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.

SELECT city,
COUNT(*) AS num_patients
FROM patients
group by city
ORDER BY num_patients DESC, city ASC;

#Show first name, last name and role of every person that is either patient or doctor. The roles are either "Patient" or "Doctor"
SELECT first_name, last_name, 'Patient' AS role FROM patients 
UNION ALL
select first_name, last_name, 'Doctor' AS role FROM doctors;

#Show all allergies ordered by popularity. Remove NULL values from query.

SELECT allergies,
COUNT(*) AS total_diagnosis
FROM patients
where allergies IS NOT NULL
GROUP BY allergies
ORDER BY total_diagnosis DESC;

#Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

SELECT first_name, last_name, birth_date
from patients
where YEAR(birth_date) between 1970 AND 1979
ORDER BY birth_date ASC;

#We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane

SELECT CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS new_name_format
from patients
ORDER BY first_name DESC;

#Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

SELECT province_id,
SUM(height) AS sum_height
FROM patients
GROUP BY province_id
HAVING sum_height >= 7000;

#Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'

SELECT (MAX(weight) - MIN(weight)) AS weight_delta
FROM patients
where last_name = 'Maroni';

#Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.

SELECT DAY(admission_date) AS day_number,
COUNT(*) AS number_of_admissions
FROM admissions
GROUP BY day_number
ORDER BY number_of_admissions DESC;

#Show all columns for patient_id 542's most recent admission_date.

SELECT * FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1;

#Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
#1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
#2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

SELECT patient_id, attending_doctor_id, diagnosis
FROM admissions
WHERE (patient_id % 2 != 0
AND attending_doctor_id IN (1, 5, 19))
or (attending_doctor_id LIKE '%2%'
AND LENGTH(patient_id) = 3);

#Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.

SELECT first_name, last_name,
COUNT(*) AS admissions_total
FROM admissions
JOIN doctors ON doctors.doctor_id = admissions.attending_doctor_id
group by attending_doctor_id;

#For each doctor, display their id, full name, and the first and last admission date they attended.
SELECT doctor_id, 
CONCAT(first_name, ' ', last_name) AS full_name,
MIN(admission_date) AS first_admission_date,
MAX(admission_date) AS last_admission_date
FROM admissions
JOIN doctors ON doctors.doctor_id = admissions.attending_doctor_id
group by doctor_id;

#Display the total amount of patients for each province. Order by descending.

SELECT province_name,
COUNT(*) AS patient_count
FROM patients
JOIN province_names ON province_names.province_id = patients.province_id
group by province_name
ORDER by patient_count DESC;

#For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

SELECT CONCAT(patients.first_name, ' ', patients.last_name) AS patient_name,
admissions.diagnosis,
CONCAT(doctors.first_name, ' ', doctors.last_name)
FROM patients
JOIN admissions ON admissions.patient_id = patients.patient_id
JOIN doctors ON doctors.doctor_id = admissions.attending_doctor_id;

#display the number of duplicate patients based on their first_name and last_name.

SELECT first_name, last_name,
COUNT(*) AS number_of_duplicates
FROM patients
group by first_name, last_name
HAVING COUNT(*) > 1;

#Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.

Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.

SELECT concat(first_name, ' ', last_name) AS patient_name, 
ROUND(height / 30.48, 1) AS 'height "Feet"',
ROUND(weight * 2.205, 0) AS 'weight "Pounds"', birth_date,
CASE gender 
WHEN 'M' THEN 'Male'
WHEN 'F' THEN 'Female'
ELSE 'Other'
END AS gender_type
FROM patients;
