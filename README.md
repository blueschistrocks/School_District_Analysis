# School District Overview Analysis

## Purpose
The purpose of the school district analysis was to clean, review and provide summary data frames of the data sets provided by the school district.  In addition, the school district asked to have the data reanalyzed due to evidence of academic dishonesty associated with the reading and math grades.

## Original Code Discussion
The analysis was conducted using Panda within a Jupiter Notebook. The two data sets (schools_complete.csv and students_complete.csv) were loaded into data two frames, reviewed for missing data and errors.  The primary issue with the data sets involved the student names in the students_complete.csv data set.  The names were indicated to have erroneous prefixes and suffixes, which were removed iterating through the names and replacing the erroneous prefixes and suffixes with an empty space using the following code:

	##List of prefixes and suffixes
	prefixes_suffixes = ["Dr. ", "Mr. ","Ms. ", "Mrs. ", "Miss ", " MD", " DDS", " DVM", " PhD"]

	#This code will iterate words in the "prefixes_suffixes" list and replace them with an empty space
	for word in prefixes_suffixes:
	    student_data_df["student_name"] = student_data_df["student_name"].str.replace(word,"")

Once the data sets were reviewed cleans the two data frames were combined using a left merge into a single DataFrameusing the following code:

	#Combines the two dateframes into a single dataset
	school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"]) 
	
### Original District Summary DataFrame	
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
![District Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/12ab668429a5d67b2d187e78c36b43c4f1720639/Resources/Orig_District_Summary.png)

### Original School Summary DataFrame
A School Summary DataFrame was created by finding the school types, total students per school, total budget per school, average math score per school, average reading score per school, precent passed math per school, percent passing reading per school and an overall percentage of passing per school. This information was placed into a DataFrame using the following code:

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

An image of the School Summary DataFrame is provided below:
![Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_School_Summary_df.png)

A chart of the School Summary Data is provide below:
![Per School_summary_chart](https://github.com/blueschistrocks/School_District_Analysis/blob/2cecb0f109a6617964de9a82077c49d01c95c4a0/Resources/Orignal_Perc_by_School_Chart.png)

### Original Top and Bottom Five Schools
Images of the DateFrame showing only the top five and bottom five schools in the district is provided below:
![Top Five Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_Top_5_Schools.png)
![Bottom Five School Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/8e734a4bb150135c4dc661f263e19e6197383686/Resources/Orig_Bottom_5_Schools.png)

### Original Math and Reading Scores by Grade
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

The DataFrames were formatted using similar formatting code use in the DataFrames discussed above.

Images of the DateFrames with average reading scores and average math scores are provided below:
![Average_Reading_by_Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Average_Reading_by_Grade.png)
![Average_Math_by_Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Average_Math_by_Grade.png)

### Original School Spending and School Size Summaries
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

The DataFrames were formatted using similar formatting code use in the DataFrames discussed above.

### Original Scores by School Type
Using Groupby series were created with the average math and reading scores by school type.  The series were merged in to a DataFrame of scores by school type.  Below is an example of the code used to create the resulting Dataframe.

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

The DataFrames were formatted using similar formatting code use in the DataFrames discussed above.

An image of the two DateFrame is provided below:
![Reading Scores_by_school type](https://github.com/blueschistrocks/School_District_Analysis/blob/e6184b1f38b739968a3b63e3b9fd153fac7c64a1/Resources/Orig_Scores_by_Type.png)

A chart of the Passing Scores By School Type is provide below:
![Reading Scores_by_school type_chart](https://github.com/blueschistrocks/School_District_Analysis/blob/31e752ced2380df08150a2318c3710ba87c3f64d/Resources/Orig_Perc_by_School_Type_Chart.png)


## Results of Original Analysis
The results of the school district analysis appear to indicate that over all the charter school students received on average higher scores in math and reading district school students.  Additionally, the schools with total students ranging from 2,000 to 5,000 appear to have the lowest math and reading scores.  Higher total spending and/or higher spending per student does not appear to be a factor in better math and reading scores as the highest scores are at the lowest spending range of less than $584 per student and the lowest scores are in the highest spending range above $645 per student. The difference between the highest spend per student and the lowest per student spend is $91.  The charter schools tended to have much lower total student than the district schools, therefore total student numbers could be a major factor in lower test scores.  

## Re-Analysis Discussion

The re-analysis of the district data sets was conducted by removing the 9th grade reading and math scores from the Thomas High School scores and replacing them with NaN. The code used to complete this is provide below. 

	##Using the loc method on the student_data_df the reading scores from the 9th grade at Thomas High School were selected and replaced with NaN.
	student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),['reading_score']] = np.nan

	##The above code was refactored to replace the math scores with NaN.
	student_data_df.loc[(student_data_df['school_name'] == 'Thomas High School') & (student_data_df['grade'] == "9th"),['math_score']] = np.nan

Using the following code, the percent passing math and reading for Thomas High School were re-calculated without the 9th grade class.  The re-calculated percent passing was added to the the per_school_summary_df.

	##Get the number of 10th-12th graders from Thomas High School (THS).
	Thomas_high_10th_df = school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School") 
						  & (school_data_complete_df["grade"] == "10th")]
	Thomas_HS_students_10th = Thomas_high_10th_df["student_name"].count()

	Thomas_high_11th_df = school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School") 
						  & (school_data_complete_df["grade"] == "11th")]
	Thomas_HS_students_11th = Thomas_high_11th_df["student_name"].count()

	Thomas_high_12th_df = school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School") 
						  & (school_data_complete_df["grade"] == "12th")]
	Thomas_HS_students_12th = Thomas_high_12th_df["student_name"].count()

	THS_10_11_12_Stud = Thomas_HS_students_10th + Thomas_HS_students_11th + Thomas_HS_students_12th
	THS_10_11_12_Stud

	##Get all the students passing math from THS
	frames = [Thomas_high_10th_df, Thomas_high_11th_df, Thomas_high_12th_df]
	THS_School_data_df = pd.concat(frames)
	passing_math_THS = THS_School_data_df[THS_School_data_df["math_score"] >= 70]
	passing_math_THS.head()

	##Get all the students passing reading from THS
	passing_reading_THS = THS_School_data_df[THS_School_data_df["reading_score"] >= 70]
	passing_reading_THS.head()

	##Get all the students passing math and reading from THS
	passing_math_reading_THS = THS_School_data_df[(THS_School_data_df["math_score"] >= 70) & (THS_School_data_df["reading_score"] >= 70)]
	passing_math_reading_THS.head()

	##Calculate the percentage of 10th-12th grade students passing math from Thomas High School. 
	passing_math_THS_count = passing_math_THS["student_name"].count()
	passing_math_percentage_THS = passing_math_THS_count / float(THS_10_11_12_Stud) * 100
	passing_math_percentage_THS

	##Calculate the percentage of 10th-12th grade students passing reading from Thomas High School.
	passing_reading_THS_count = passing_reading_THS["student_name"].count()
	passing_reading_percentage_THS = passing_reading_THS_count / float(THS_10_11_12_Stud) * 100
	passing_reading_percentage_THS

	##Calculate the overall passing percentage of 10th-12th grade from Thomas High School. 
	passing_math_reading_THS_count = passing_math_reading_THS["student_name"].count()
	passing_math_reading_percentage_THS = passing_math_reading_THS_count / float(THS_10_11_12_Stud) * 100
	passing_math_reading_percentage_THS	

	##Replace the passing math percent for Thomas High School in the per_school_summary_df.
	per_school_summary_df.loc[(per_school_summary_df['Total Students'] == 1635), "% Passing Math"] = passing_math_percentage_THS
	#df.loc[(df.Event == 'Dance'),'Event']='Hip-Hop'

	##Replace the passing reading percentage for Thomas High School in the per_school_summary_df.
	per_school_summary_df.loc[(per_school_summary_df['Total Students'] == 1635),
				  "% Passing Reading"] = passing_reading_percentage_THS

	##Replace the overall passing percentage for Thomas High School in the per_school_summary_df.
	per_school_summary_df.loc[(per_school_summary_df['Total Students'] == 1635),
				  "% Overall Passing"] = passing_math_reading_percentage_THS
	

### Re-Analysis District Summary
The effect of the removal of the scores for the 9th grade class on the District Summary was viable but negligible.

![Re-Analysis District Summary](https://github.com/blueschistrocks/School_District_Analysis/blob/07aa8d6b447af22ba91923f6dd1240906a1b3ee2/Resources/District_Summary_df.png)

## Re-Analysis School Summary 
Since the Thomas High School percentage were recalculated without the 9th grade class there was no effect on the school summary as a whole.

![Re-Analysis School Summary](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/School_Summary_df.png)

A chart of the Per School Summary Data prior to the Thomas High School Recalculation is provide below:
![Per School Summary type_chart](https://github.com/blueschistrocks/School_District_Analysis/blob/f5e6d668618ad59dafa645642c7fee2b295d2aa4/Resources/Perc_by_School_Chart_B_RE.png)

A chart of the Per School Summary Data after the Thomas High School Recalculation is provide below:
![Per School Summary type_chart](https://github.com/blueschistrocks/School_District_Analysis/blob/f5e6d668618ad59dafa645642c7fee2b295d2aa4/Resources/Perc_by_School_Chart_a_RE.png)


 ### Re-Analysis Top and Bottom Schools
 ![Top Five Scool Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Top_5_Schools.png)
![Bottom Five School Summary DF](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Bottom_5_Schools.png)
 

### Effect on Thomas High School 
Since the math and reading scores were re-calculated the effect on Thomas Hight School was negligible.  However, if the scores were not recalculated the affect would have been significant, see below for the Thomas High School Scores without the 9th grade class prior to the recalculation.
![image](https://github.com/blueschistrocks/School_District_Analysis/blob/5b3604d5cf8a72544905f8617c70cf9c899ce176/Resources/THS_before_recal.png)

## Effect of 9th Grade Math and Reading Score Removal and Math and Reading Scores by Grade
The effect of the removal of the scores for the 9th grade class was that there were no grades to analyze therefore no information could be obtained regarding the 9th grade class.  Math and Reading scores by other grades at Thomas High School or in all grades in other schools were not affected due to the re-calculation. 

![M-by Grade](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Average_Math_by_Grade.png)
![reading by Grade ](https://github.com/blueschistrocks/School_District_Analysis/blob/a15a3a14f1ad207569b22df3e98119b5a49d110a/Resources/Average_Reading_by_Grade.png)


### Scores by School Spending
Scores by school spending were not affected after recalculation.
![Scores by School Spending](https://github.com/blueschistrocks/School_District_Analysis/blob/2cecb0f109a6617964de9a82077c49d01c95c4a0/Resources/Scores_by_Spending.png)

### Scores by School Size
Scores by school size were not affected after recalculation.
![Scores by School Size](https://github.com/blueschistrocks/School_District_Analysis/blob/2cecb0f109a6617964de9a82077c49d01c95c4a0/Resources/Scores_by_Size.png)


### Scores by School Type
Scores by school Type were not affected after recalculation.
![Scores by School Type](https://github.com/blueschistrocks/School_District_Analysis/blob/2cecb0f109a6617964de9a82077c49d01c95c4a0/Resources/Scores_by_Type.png)


## Summary
The removal of the 9th grade math and reading scores significantly reduced the percent passing of the math, reading and overall percent passing for the entire school.  If not for the re-calculation Thomas High School would have lost its spot in the top five schools in the district.
The removal of the scores also affected a minor but negligible change in the district percent passing.  


