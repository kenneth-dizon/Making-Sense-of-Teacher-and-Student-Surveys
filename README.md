# Making-Sense-of-Teacher-and-Student-Surveys

![pic](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/4aff2ca1-1ddd-42da-9336-2aa91ea92a37)


In this case study, I assisted the fictional Mercy Public Schools in analyzing survey data from students and teachers to recommend a new evaluation model.   I used SQL and Tableau. The current evaluation model shown below needs improvement since many teachers express dislike.

![mercy](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/6a84c2b4-bd69-4f39-90d3-de7f800acebd)

Data from the surveys could not be shared  since the organization I did this case study for only gave me permission to publish my process. The surveys contain hundreds of student and teacher responses. 

## Table of Contents
- [Data Preparation](#Data-Preparation)
- [Data Analysis and Visualization](#Data-Analysis-and-Visualization)
- [Recommendations: Choosing a New Evaluation Model](#ecommendations:-Choosing-a-New-Evaluation-Model)

 ## Data Preparation
 Using PostgreSQL 15, I first created the tables for the teacher and student surveys. I set “ResponseID” as the primary key in the schemas for both surveys. Below is the schema for the teacher survey. The schema for the student survey looks similar but with different columns.


![teacher schema](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/93ab664e-ed98-45ba-ab12-b919a834f71f)

 Once done, I imported the CSV files of the teacher and student surveys to populate the tables.
 
 ![teacher survey example](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/87034146-b696-4482-8c16-4e6415235321)
 
![student_example ](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/14bc7a29-c0c7-4c93-9052-487b4b42ee4f)

Considering that a “1” equates to “Strongly Disagree” and a “4” to “Strongly Agree”, I reformated both tables using the CASE function to create new ones where all the ratings are integers.

````sql
CREATE TABLE teacher_cleaned AS SELECT * FROM 
(SELECT "ResponseID", "Survey Type", "I have a sense of self-efficacy for instructional strategies", 
"State tests are a good lever in determining the quality of a te", "I work in a collegial work environment", 
"I feel supported by the current administration", "Parents and students are engaged",
CASE 
 WHEN "Being observed by the principal annually is a sufficient way to" = 'Stongly Agree' THEN 4 
 WHEN "Being observed by the principal annually is a sufficient way to" = 'Agree' THEN 3 
 WHEN "Being observed by the principal annually is a sufficient way to" = 'Disagree' THEN 2 
 ELSE 1
 END "Being observed by the principal annually is a sufficient way to measure success",
CASE 
 WHEN "I am satisfied with our current evaluation method" = 'Stongly Agree' THEN 4 
 WHEN "I am satisfied with our current evaluation method" = 'Agree' THEN 3 
 WHEN "I am satisfied with our current evaluation method" = 'Disagree' THEN 2 
 ELSE 1
 END "I am satisfied with our current evaluation method"
FROM public.teacher) AS teacher
````

For the student table, I changed all null values to a text string using the COALESCE function.

````sql
CREATE TABLE student_cleaned AS SELECT * FROM
(SELECT "ResponseID", "Survey Type", 
 COALESCE("Ethnicity", 'unspecified') "Ethnicity",
 COALESCE("Learning Needs", 'Non-ELL') "Learning Needs",
 COALESCE("Special Education", 'Non-SPED') "Special Education", 
 "My teachers are effective", "My principal is effective", “My school is a positive environment”,
CASE  
 WHEN "My academic growth is a good indicator of my teacher's ability" = 'Stongly Agree' THEN 4 
 WHEN "My academic growth is a good indicator of my teacher's ability" = 'Agree' THEN 3 
 WHEN "My academic growth is a good indicator of my teacher's ability" = 'Disagree' THEN 2 
 ELSE 1
 END "My academic growth is a good indicator of my teacher's ability",
CASE 
 WHEN "My voice is important in determining teacher effectiveness" = 'Stongly Agree' THEN 4 
 WHEN "My voice is important in determining teacher effectiveness" = 'Agree' THEN 3 
 WHEN "My voice is important in determining teacher effectiveness"= 'Disagree' THEN 2 
 ELSE 1
 END "My voice is important in determining teacher effectiveness"  
FROM public.student) as student
````

I then saved the results of both tables as CSV files.

## Data Analysis and Visualization
After uploading the CSV files to Tableau, I created dashboards that reflected the results of the teacher and student surveys.

![teacher dashboard](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/89b5f3c0-3392-484a-b980-cb407730aa8e)

Here are key takeaways from the teacher survey:

- 62.07% of teachers disagree or strongly disagree with the statement: “I am satisfied with our current evaluation method.” This confirms that Mercy Public Schools needs to change its evaluation model.
- 77.01% of teachers disagree or strongly disagree with the statement: “Being observed by the principal annually is a sufficient way to measure success.” This means that the new model should exclude principal evaluation.
- 62.07% of teachers agree or strongly agree with the statement: “State tests are a good lever in determining the quality of a teacher.” This means that the new model should include student test scores.


![student dashboard](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/48e5c588-655f-4676-b4ab-cf2e68256b09)

The student survey offers key insights:

- 98.2% of students agree or strongly agree with the statement: “My academic growth is a good indicator of my teacher’s ability”. Like the findings from the teacher survey, this finding supports that student test scores should be included.
- 80.63% of students agree or strongly agree with the statement: “My voice is important in determining teacher effectiveness.” This means that the new model should student feedback.

## Recommendations: Choosing a New Evaluation Model

Again, Mercy Public School’s current evaluation system is as follows.

![mercy](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/b3786428-8565-4eca-8acd-6c34b357bc5b)

The superintendent wants my opinion on which of the following evaluation models they should adopt. I could not alter any of the models.


![elm](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/5ebc9af4-83a6-404f-8668-e57f5cb4c8f1)

![redwood](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/afecd8d8-2486-49ff-b398-6ec9154c947c)


![birch](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/414c1f5c-fd41-415c-8295-764add1217d9)

![oak](https://github.com/kenneth-dizon/Making-Sense-of-Teacher-and-Student-Surveys/assets/141383645/f6c91d24-53e6-4185-8a74-9a5f6ca2f0ba)

Immediately, we could eliminate Elm Public School’s model since it does not include student feedback, which students feel is integral in improving their teacher’s performance. We could also eliminate Redwood Public School’s evaluation model since it includes principal evaluation, a method many teachers dislike at Mercy.

That leaves us with the Birch and Oak models. Both include state exams as an evaluation method and bi-annual student surveys. Let’s look at the other evaluation metrics to differentiate both models.

Unlike Oak, Birch includes parent feedback. Though it may be important, we must discount it for now since we do not have evidence from the survey results that supports its implementation. In future surveys, it would be helpful to ask questions about parent involvement.

Let’s look at the metric both models share, teacher growth and development. Birch’s model includes “observations conducted by principal and/or peers.” Since survey results clearly point out that teachers dislike principal observation, the only way we would implement Birch’s model is if use peer observation.

When we look at Oak’s model, we see a different approach. It includes “bi-annual classroom observations conducted by [an] outside team of trained evaluators.” Teachers would receive feedback from a knowledgeable third party, not their boss. They would also be measured by their “portfolios of teacher work-curriculum development.” Since 82.76% of teachers agree or strongly agree with the statement: “I have a sense of self-efficacy for instructional strategies”, portfolios would be a great way to measure teacher performance since it promotes agency amongst teachers in implementing their curriculum.

Though Birch’s model holds promise, Oak’s model is a breath of fresh air. Oak’s model excludes principal evaluation and includes student surveys, state test scores, and a new evaluation method — teacher portfolios.

Overall, Oak’s evaluation model responds to the needs of teachers and students in Mercy Public Schools. Therefore, I recommend that the superintendent of Mercy Public School adopt Oak’s model.












