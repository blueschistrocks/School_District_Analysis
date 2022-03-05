# School District Overview Analysis

## Purpose
The purpose of the school district analysis was to clean, review and provide summary data frames of the data sets provided by the school district.  In addition, the school district asked to have the data reanalyzed due to evidence of academic dishonesty associated with the reading and math grades.

## Code Discussion
The analysis was conducted using Panda within a Jupiter Notebook. The two data sets (schools_complete.csv and students_complete.csv) were loaded into data two frames, reviewed for missing data and errors.  The primary issue with the data sets involved the student names in the students_complete.csv data set.  The names were indicated to have erroneous prefixes and suffixes, which were removed iterating through the names and replacing the erroneous prefixes and suffixes with an empty space using the following code:

	##List of prefixes and suffixes
	prefixes_suffixes = ["Dr. ", "Mr. ","Ms. ", "Mrs. ", "Miss ", " MD", " DDS", " DVM", " PhD"]

	#This code will iterate words in the "prefixes_suffixes" list and replace them with an empty space
	for word in prefixes_suffixes:
	    student_data_df["student_name"] = student_data_df["student_name"].str.replace(word,"")

Once the data sets were reviewed cleans the two data frames were combined using a left merge into a single DataFrameusing the following code:

	#Combines the two dateframes into a single dataset
	school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"]) 

Using the merged data frame, a summary DataFrame of the school district data was created by finding the total school count, total students, total budget, average math score, average reading score, precent passed math, percent passing reading and a overall percentage of passing. This information was placed into a DataFrame using the following code:

	#District summary DataFrame 
	district_summary_df = pd.DataFrame(
		  [{"Total Schools": school_count, 
		  "Total Students": student_count, 
		  "Total Budget": total_budget,
		  "Average Math Score": average_math_score, 
		  "Average Reading Score": average_reading_score,
		  "% Passing Math": passing_math_percentage,
		 "% Passing Reading": passing_reading_percentage,
		"% Overall Passing": overall_passing_percentage}])

The district summary DataFrame was formatted using the following code:

	#Formating code
	district_summary_df["Total Students"] = district_summary_df["Total Students"].map("{:,}".format)
	district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map("${:,.0f}".format)
	district_summary_df["Average Math Score"] = district_summary_df["Average Math Score"].map("{:.1f}".format)
	district_summary_df["Average Reading Score"] = district_summary_df["Average Reading Score"].map("{:.1f}".format)
	district_summary_df["% Passing Math"] = district_summary_df["% Passing Math"].map("{:.1f}".format)
	district_summary_df["% Passing Reading"] = district_summary_df["% Passing Reading"].map("{:.1f}".format)
	district_summary_df["% Overall Passing"] = district_summary_df["% Overall Passing"].map("{:.1f}".format)

An image of the district summary DataFrame is provided below:
![District Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_District_Summary.png)


A Per School Summary DataFrame was created by finding the school types, total students per school, total budget per school, average math score per school, average reading score per school, precent passed math per school, percent passing reading per school and an overall percentage of passing per school. This information was placed into a DataFrame using the following code:

	#Per school summary data frame
	per_school_summary_df = pd.DataFrame({
	    "School Type": per_school_types,
	    "Total Students": per_school_counts,
	    "Total School Budget": per_school_budget,
	    "Per Student Budget": per_school_capita,
	    "Average Math Score": per_school_math,
	    "Average Reading Score": per_school_reading,
	    "% Passing Math": per_school_passing_math,
	    "% Passing Reading": per_school_passing_reading,
	    "% Overall Passing": per_overall_passing_percentage})

The Per School Summary DataFrame was formatted using the following code:


	#Formating code
	per_school_summary_df["Total School Budget"] = per_school_summary_df["Total School Budget"].map("${:,.0f}".format)
	per_school_summary_df["Per Student Budget"] = per_school_summary_df["Per Student Budget"].map("${:,.0f}".format)
	per_school_summary_df["Total Students"] = per_school_summary_df["Total Students"].map("{:,.0f}".format)
	per_school_summary_df["Average Math Score"] = per_school_summary_df["Average Math Score"].map("{:.1f}".format)
	per_school_summary_df["Average Reading Score"] = per_school_summary_df["Average Reading Score"].map("{:.1f}".format)
	per_school_summary_df["% Passing Math"] = per_school_summary_df["% Passing Math"].map("{:.0f}".format)
	per_school_summary_df["% Passing Reading"] = per_school_summary_df["% Passing Reading"].map("{:.0f}".format)
	per_school_summary_df["% Overall Passing"] = per_school_summary_df["% Overall Passing"].map("{:.0f}".format)
	per_school_summary_df

An image of the Per School summary DataFrame is provided below:
![Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_School_Summary_df.png)

Images of the DateFrame showing only the top five and bottom five schools in the district is provided below:
![Top Five Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_Top_5_Schools.png)
![Bottom Five School Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_Bottom_5_Schools.png)

Using conditionals and Groupby series were created with the average math and reading scores by grade and school.  The series were merged in to DataFrames, one with math scores by grade and school and one with reading scores by grade and school. Below is an example of the code used to create the resulting Dataframes.

	#Creates a Series of scores by grade levels using conditionals.
	ninth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "9th")]
	tenth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "10th")]
	eleventh_graders = school_data_complete_df[(school_data_complete_df["grade"] == "11th")]
	twelfth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "12th")]

	#Groups each school Series by the school name for the average math score.
	ninth_grade_math_scores = ninth_graders.groupby(["school_name"]).mean()["math_score"]
	tenth_grade_math_scores = tenth_graders.groupby(["school_name"]).mean()["math_score"]
	eleventh_grade_math_scores = eleventh_graders.groupby(["school_name"]).mean()["math_score"]
	twelfth_grade_math_scores = twelfth_graders.groupby(["school_name"]).mean()["math_score"]

	#Group each school Series by the school name for the average reading score.
	ninth_grade_reading_scores = ninth_graders.groupby(["school_name"]).mean()["reading_score"]
	tenth_grade_reading_scores = tenth_graders.groupby(["school_name"]).mean()["reading_score"]
	eleventh_grade_reading_scores = eleventh_graders.groupby(["school_name"]).mean()["reading_score"]
	twelfth_grade_reading_scores = twelfth_graders.groupby(["school_name"]).mean()["reading_score"]

	#Combines each Series for average math scores by school into single data frame.
	math_scores_by_grade = pd.DataFrame({
	reading_scores_by_grade = pd.DataFrame({
		      "9th": ninth_grade_reading_scores,
		      "10th": tenth_grade_reading_scores,
		      "11th": eleventh_grade_reading_scores,
		      "12th": twelfth_grade_reading_scores})

	#Combines each Series for average reading scores by school into single data frame.
	reading_scores_by_grade = pd.DataFrame({
		      "9th": ninth_grade_reading_scores,
		      "10th": tenth_grade_reading_scores,
		      "11th": eleventh_grade_reading_scores,
		      "12th": twelfth_grade_reading_scores})

The DataFrames were formatted using similar formatting code as shown is ?? above.

Images of the DateFrames with average reading scores and average math scores are provided below:
![Average_Reading_by_Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Average_Reading_by_Grade.png)
![Average_Math_by_Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Average_Math_by_Grade.png)

Two Dataframes in the images below were created using Bin and Groupby to group the math and reading scores and percentages by spending ranges and by school size.
![Scores_by_Spending](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Scores_by_Spending.png)
![Scores_by_Size](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Scores_by_Size.png)

Below is an example of the code used to create the resulting Dataframes.

	#Establishing the bins.
	size_bins = [0, 1000, 2000, 5000]
	group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
	#Categorize spending based on the bins.
	per_school_summary_df["Total Students"] = per_school_summary_df["Total Students"].str.replace(',','')
	per_school_summary_df["Total Students"] = per_school_summary_df["Total Students"].astype(float)
	per_school_summary_df["School Size"] = pd.cut(per_school_summary_df["Total Students"], size_bins, labels=group_names)

The DataFrames were formatted using similar formatting code as shown is ?? above.

Using Groupby series were created with the average math and reading scores by school type.  The series were merged in to a DataFrame.  Below is an example of the code used to create the resulting Dataframe.

	#Calculates averages for the desired columns. 
	type_math_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Math Score"]
	type_reading_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Reading Score"]
	type_passing_math = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Math"]
	type_passing_reading = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Reading"]
	type_overall_passing = per_school_summary_df.groupby(["School Type"]).mean()["% Overall Passing"]

	#Assembles the series into a DataFrame. 
	type_summary_df = pd.DataFrame({
		  "Average Math Score" : type_math_scores,
		  "Average Reading Score": type_reading_scores,
		  "% Passing Math": type_passing_math,
		  "% Passing Reading": type_passing_reading,
		  "% Overall Passing": type_overall_passing})

The DataFrame was formatted using similar formatting code as shown is ?? above.

Images of the two DateFrames provided below:
![Reading Scores_by_school type](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Scores_by_Type.png)

## Results
The results of the school district analysis appear to indicate that over all the charter school students received on average higher scores in math and reading district school students.  Additionally, the schools with total students ranging from 2,000 to 5,000 appear to have the lowest math and reading scores.  Higher total spending and/or higher spending per student does not appear to be a factor in better math and reading scores as the highest scores are at the lowest spending range of less than $584 per student and the lowest scores are in the highest spending range above $645 per student. The difference between the highest spend per student and the lowest per student spend is $91.  The charter schools tended to have much lower total student than the district schools, therefore total student numbers could be a major factor in lower test scores.  ![image](https://user-images.githubusercontent.com/89749126/156869079-4c55d89d-7a64-45ed-8fe1-dccd88eb24cf.png)

## Re-Analysis Discussion




## Effect of 9th Grade Math and Reading Score Removal

### Re-Analysis District Summary

![Re-Analysis District Summary](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/District_Summary_df.png)

## Re-Analysis School Summary 

![Re-Analysis School Summary](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/School_Summary_df.png)
 ### Re-Analysis Top and Bottom Schools
 ![Top Five Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Top_5_Schools.png)
![Bottom Five School Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Bottom_5_Schools.png)
 

- Effect on Thomas High School 

### Math and Reading Scores by Grade
 ![M-by Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Average_Math_by_Grade.png)
![reading by Grade ](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Average_Reading_by_Grade.png)

	
	- Scores by School Spending

- Scores by School Size

	- Scores by School Type

## Summary
