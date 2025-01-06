# Converting the dataset to an R-friendly format and saving it for analysis in R
r_friendly_path = "/mnt/data/students_survey_r_analysis.csv"
data.to_csv(r_friendly_path, index=False)

r_analysis_instructions = """
# R Analysis Instructions:
# 1. Import the dataset using read.csv().
# 2. Perform exploratory data analysis (EDA) as per the given questions.
# 3. Utilize R libraries like ggplot2, dplyr, and summarytools for detailed insights.

# Load dataset
survey_data <- read.csv('{}')

# Questions and corresponding R code:

# Numerical Variables:
# 1. Distribution patterns for numerical variables
summary(survey_data[c("Age", "high_school_grade", "study_hours", "sleeping_hours", "mid_score", "recommend_university")])

# 2. Outliers in numerical variables
boxplot(survey_data$high_school_grade, main="High School Grade Outliers")
boxplot(survey_data$study_hours, main="Study Hours Outliers")
boxplot(survey_data$sleeping_hours, main="Sleeping Hours Outliers")

# 3. Correlation between study_hours and mid_score
cor(survey_data$study_hours, survey_data$mid_score, use="complete.obs")

# 4. Age variation with high_school_grade or GPA
plot(survey_data$Age, survey_data$high_school_grade, main="Age vs High School Grade")
plot(survey_data$Age, as.numeric(as.character(survey_data$gpa)), main="Age vs GPA")

# 5. Average university_satisfaction by GPA
aggregate(university_satisfaction ~ gpa, data = survey_data, mean)

# 6. Sleeping_hours vs university_satisfaction
cor(survey_data$sleeping_hours, survey_data$university_satisfaction, use="complete.obs")

# 7. Trends between mid_score and recommend_university
plot(survey_data$mid_score, survey_data$recommend_university, main="Mid Score vs Recommend University")

# Categorical Variables:
# 1. Distribution of categorical variables
table(survey_data$Gender)
table(survey_data$year)
table(survey_data$work)

# 2. University satisfaction across high_school_category
aggregate(university_satisfaction ~ high_school_category, data = survey_data, mean)

# 3. Distance_to_uni and recommend_university scores
aggregate(recommend_university ~ distance_to_uni, data = survey_data, mean)

# 4. Business owners vs employees
table(survey_data$business_owner)
table(survey_data$type_of_work)

# 5. Gender and work type on bida_satisfaction
aggregate(bida_satisfaction ~ Gender + work, data = survey_data, mean)

# 6. Most common top_concerns
library(stringr)
concerns <- unlist(strsplit(as.character(survey_data$top_concerns), ";"))
table(concerns)

# 7. Recommend_bida by high_school_category
aggregate(recommend_bida ~ high_school_category, data = survey_data, mean)

# Mixed Analysis:
# 1. Mid_score and GPA variation by year
aggregate(mid_score ~ year, data = survey_data, mean)
aggregate(as.numeric(as.character(gpa)) ~ year, data = survey_data, mean)

# 2. Mid_score by work status
aggregate(mid_score ~ work, data = survey_data, mean)

# 3. Distance_to_uni and study_hours/sleeping_hours
aggregate(study_hours ~ distance_to_uni, data = survey_data, mean)
aggregate(sleeping_hours ~ distance_to_uni, data = survey_data, mean)

# 4. Gender and satisfaction levels
aggregate(university_satisfaction ~ Gender, data = survey_data, mean)
aggregate(bida_satisfaction ~ Gender, data = survey_data, mean)

# 5. High_school_category and GPA/mid_score
aggregate(mid_score ~ high_school_category, data = survey_data, mean)
aggregate(as.numeric(as.character(gpa)) ~ high_school_category, data = survey_data, mean)

# Practical Business Insights:
# 1. Factors most associated with university satisfaction
library(corrplot)
numeric_cols <- survey_data[sapply(survey_data, is.numeric)]
corrplot(cor(numeric_cols, use="complete.obs"), method="circle")

# 2. Feedback from recommend_university and recommend_bida
summary(survey_data$recommend_university)
summary(survey_data$recommend_bida)

# 3. Working students vs non-working students satisfaction
aggregate(university_satisfaction ~ work, data = survey_data, mean)

# 4. Groups needing support based on GPA and satisfaction
aggregate(cbind(as.numeric(as.character(gpa)), university_satisfaction) ~ year, data = survey_data, mean)

# Save Results or Visualizations
# Use write.csv() for tables and ggsave() for ggplot visualizations.
""".format(r_friendly_path)

# Save the instructions to a text file for R
instructions_path = "/mnt/data/r_analysis_instructions.txt"
with open(instructions_path, "w") as file:
    file.write(r_analysis_instructions)

instructions_path
