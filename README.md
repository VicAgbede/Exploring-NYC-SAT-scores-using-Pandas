# Exploring-NYC-SAT-scores-using-Pandas

# Import  pandas
import pandas as pd

# Read in the data
schools = pd.read_csv("schools.csv")

# Preview the data
schools.head()


# Finding schools with the best math scores (80% or greater of the 800 maximum points possible)
best_math_scores = schools[schools['average_math'] >= 640].sort_values('average_math', ascending = False)
#Subset columns for the schools with the best math scores and their scores
best_math_schools = best_math_scores[['school_name', 'average_math']]
#print(best_math_schools)



# Finding the top 10 performing schools based on the combined SAT 

# Add a new column named total_SAT that contains scores of all three subjects obtained at the SAT exam for each school
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']
                                                                                    
# Sort the total_SAT scores in descending order
schools_srt_SAT = schools.sort_values('total_SAT', ascending = False)
                                      
# Subset columns for the top ten scores with their corresponding schools
top_10_schools = schools_srt_SAT[['school_name', 'total_SAT']].head(10)
#print(top_10_schools)



# Filtering the data for the borough with the largest standard deviation
# Count the number of schools in each borough
num_schools = schools['borough'].value_counts()
                                      
# Find the average SAT scores for each borough and round up to two decimal places
average_SAT = schools.groupby('borough')['total_SAT'].agg(['mean']).round(2)
                                      
# Find the standard deviation of the SAT scores for each borough and round up to two decimal places
std_SAT = schools.groupby('borough')['total_SAT'].agg(['std']).round(2)
                                      
# Find the name of the borough with the largest standard deviation
largest_std_dev_borough = std_SAT['std'].idxmax()
                                      
# Find the std of the borough with the largest standard deviation
largest_std_dev_score = std_SAT.loc[largest_std_dev_borough, 'std']
print(largest_std_dev_score)



# Creating a DataFrame to display the results (the name, number of schools, average and std SAT score) for the borough with the largest standard deviation
largest_std_dev = pd.DataFrame({
    "borough": [largest_std_dev_borough],
    "num_schools": [num_schools[largest_std_dev_borough]],
    "average_SAT": [average_SAT.loc[largest_std_dev_borough, 'mean']],
    "std_SAT": [largest_std_dev_score]
})

print(largest_std_dev)
