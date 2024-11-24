# Pymaceuticals_challenge

# Analysis
After completing the analysis of the data, there are several observations that can be explored in terms of the effectiveness of the different Regimens offered in this study.  The first being that the drug Capomulin has the most positive correlation when it comes to assessing the mouse's weights against the tumor volumes.  This is particularly evident when analyzing the data for other drugs in the study such as Ramicane, Infubinol, Ceftamin and in particular, when weighted against the Placebo group. This may suggest that Capomulin drug does not have much of an effect on the tumor volume of the mice because the tumor volume increases with the size of the mouse. 

![image](https://github.com/user-attachments/assets/dca2fbf9-9301-4504-b978-c26fa4943042)


During the analysis, there were minimal outliers detected, which is a positive marker as outliers could suggest abnormalities in the data which would need to be explored future.  Remaining constant means the study was likely very controlled, which is also evident in the division between the Sex of the mice being essentially even. 

![image](https://github.com/user-attachments/assets/add116b1-720e-49d7-8b9f-28ab0d775c43)


When comparing the effectiveness of the drugs Capomulin & Ramicane against Infubinol & Ceftamin, you can see the average tumor volumes are significantly higher for Infubinol & Ceftamin, this would suggest that the effectiveness of the drug is likely low as their data more closely relates to the placebo study as shown in this additional box graph created to support this hypothesis.  Due to this correlation, it would suggest that Capomulin & Ramicane are the more effective drugs at supressing average tumor volumes.  

![image](https://github.com/user-attachments/assets/db542224-70df-4427-b301-e3748dd0d024)


# Dependencies and Setup
    import matplotlib.pyplot as plt

    import pandas as pd

    import scipy.stats as st

#Study data files

    mouse_metadata_path = "data/Mouse_metadata.csv"

    study_results_path = "data/Study_results.csv"

#Read the mouse data and the study results

    mouse_metadata = pd.read_csv(mouse_metadata_path, quotechar= '"')

    study_results = pd.read_csv(study_results_path)

#Combine the data into a single DataFrame

    combined_mouse_study_data = pd.merge(study_results,mouse_metadata, on="Mouse ID")

#Display the data table for preview

    combined_mouse_study_data.head()

![image](https://github.com/user-attachments/assets/98819435-47ea-4509-ae97-b5b55170c4a1)

# Checking the number of mice.

    mouse_total = combined_mouse_study_data["Mouse ID"].nunique()

    mouse_total

![image](https://github.com/user-attachments/assets/9edbe205-fd51-46d0-9e02-2171d533dbb0)

# Our data should be uniquely identified by Mouse ID and Timepoint
#Get the duplicate mice by ID number that shows up for Mouse ID and Timepoint.

#Optional: Get all the data for the duplicate mouse ID.

    duplicate_mouse_data = combined_mouse_study_data[combined_mouse_study_data.duplicated(subset=['Mouse ID','Timepoint'], keep=False)]

    duplicate_mouse_data

![image](https://github.com/user-attachments/assets/f977b166-5c91-4278-ac55-a673cfbfb237)

# Create a clean DataFrame by dropping the duplicate mouse by its ID.
    combined_mouse_study_data = combined_mouse_study_data[combined_mouse_study_data['Mouse ID'] != 'g989']

    combined_mouse_study_data.head()

![image](https://github.com/user-attachments/assets/52514dcc-51e1-4962-93fc-37714a24e185)

# Checking the number of mice in the clean DataFrame.
    mouse_total_clean = combined_mouse_study_data["Mouse ID"].nunique()

    mouse_total_clean

![image](https://github.com/user-attachments/assets/608d5c74-50f3-4d17-8f38-05538a175be9)

# Summary Statisctics

# Generate a summary statistics table of mean, median, variance, standard deviation, and SEM of the tumor volume for each regimen
#Use groupby and summary statistical methods to calculate the following properties of each drug regimen:

#mean, median, variance, standard deviation, and SEM of the tumor volume.

#Assemble the resulting series into a single summary DataFrame.

    mouse_summary_stats = combined_mouse_study_data.groupby('Drug Regimen')['Tumor Volume (mm3)'].agg(mean='mean', median='median', variance='var', std_dev='std', sem='sem')

    mouse_summary_stats.rename(columns={'mean': 'Mean Tumor Volume','median': 'Median Tumor Volume','variance': 'Tumor Volume Variance','std_dev': 'Tumor Volume Std. Dev.','sem': 'Tumor Volume Std. Err'}, inplace=True)

    mouse_summary_stats

![image](https://github.com/user-attachments/assets/f0cab4cf-90b5-4d07-ad95-1ffe7b3c8465)

#A more advanced method to generate a summary statistics table of mean, median, variance, standard deviation, and SEM of the tumor volume for each regimen (only one method is required in the solution)

#Using the aggregation method, produce the same summary statistics in a single line

      mouse_summary_stats = combined_mouse_study_data.groupby('Drug Regimen')['Tumor Volume (mm3)'].agg(
        Mean_Tumor_Volume='mean',
        Median_Tumor_Volume='median',
        Tumor_Volume_Variance='var',
        Tumor_Volume_Std_Dev='std',
        Tumor_Volume_Std_Err='sem'
    )
    
    mouse_summary_stats

![image](https://github.com/user-attachments/assets/12cfec9c-a921-40de-a073-b721d0e969cd)

# Bar and Pie Charts

# Generate a bar plot showing the total number of rows (Mouse ID/Timepoints) for each drug regimen using Pandas.
    mouse_bar_chart = combined_mouse_study_data.groupby('Drug Regimen').size()

    mouse_bar_chart_sort = mouse_bar_chart_sort.sort_values(ascending=False)
    
    mouse_bar_chart_sort.plot(kind='bar', color = 'blue')
    
    plt.xlabel("Drug Regimen")
    
    plt.ylabel("# of Observed Mouse Timepoints")
    
    plt.show()

![image](https://github.com/user-attachments/assets/c84f50cb-d5a3-44c5-9dc7-2f4dcdb4c9f4)

# Generate a bar plot showing the total number of rows (Mouse ID/Timepoints) for each drug regimen using pyplot.

    mouse_bar_chart = combined_mouse_study_data.groupby('Drug Regimen').size()

    mouse_bar_chart_sort = mouse_bar_chart_sort.sort_values(ascending=False)
    
    plt.bar(mouse_bar_chart_sort.index, mouse_bar_chart_sort.values, color='blue')
    
    plt.xlabel("Drug Regimen")
    
    plt.ylabel("# of Observed Mouse Timepoints")
    
    plt.xticks(rotation=45)
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/03b011d5-65ac-4308-8c66-50d3c570a92c)

# Generate a pie chart, using Pandas, showing the distribution of unique female versus male mice used in the study
    mouse_gender = combined_mouse_study_data.drop_duplicates(subset='Mouse ID')[['Mouse ID', 'Sex']]

#Get the unique mice with their gender

    mouse_gender_count = mouse_gender['Sex'].value_counts()

#Make the pie chart

    mouse_gender_count.plot(kind='pie', autopct='%1.1f%%', colors=['blue', 'pink'], startangle=0)
    
    plt.show()

![image](https://github.com/user-attachments/assets/db49d0b9-2966-4651-af9a-511c947dcfb2)

# Generate a pie chart, using pyplot, showing the distribution of unique female versus male mice used in the study

    mouse_gender = combined_mouse_study_data.drop_duplicates(subset='Mouse ID')[['Mouse ID', 'Sex']]

#Get the unique mice with their gender

    mouse_gender_count = mouse_gender['Sex'].value_counts()

#Make the pie chart

    plt.pie(mouse_gender_count, labels=mouse_gender_count.index, autopct='%1.1f%%', colors=['blue', 'pink'], startangle=0)

    plt.show()

![image](https://github.com/user-attachments/assets/2b49dcb2-86e5-4f22-843b-42a2544c3963)

# Quartiles, Outliers and Boxplots

# Calculate the final tumor volume of each mouse across four of the treatment regimens: Capomulin, Ramicane, Infubinol, and Ceftamin
    treatment_regimens = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']
    
    subset_regimens = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'].isin(treatment_regimens)]

#Start by getting the last (greatest) timepoint for each mouse

    last_timepoint_df = subset_regimens.groupby('Mouse ID')['Timepoint'].max()
    
    final_tumor_volume_df = pd.merge(last_timepoint_df, combined_mouse_study_data, on=['Mouse ID', 'Timepoint'])

#Merge this group df with the original DataFrame to get the tumor volume at the last timepoint

    final_tumor_volume_df = final_tumor_volume_df[['Mouse ID', 'Drug Regimen', 'Tumor Volume (mm3)']]
    
    final_tumor_volume_df

![image](https://github.com/user-attachments/assets/d597cc5f-b975-417f-a418-f81166c61fb3)

# Put treatments into a list for for loop (and later for plot labels)
    treatment_regimens = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']

#Create empty list to fill with tumor vol data (for plotting)

    tumor_vol_list = []
    
    for treatment in treatment_regimens:
        
        treatment_regimens_data = final_tumor_volume_df[final_tumor_volume_df['Drug Regimen'] == treatment]
        
        tumor_vol_list.append(treatment_regimens_data['Tumor Volume (mm3)'])

#Calculate the IQR and quantitatively determine if there are any potential outliers.

        Q1 = treatment_regimens_data['Tumor Volume (mm3)'].quantile(0.25)
        
        Q3 = treatment_regimens_data['Tumor Volume (mm3)'].quantile(0.75)
        
        IQR = Q3 - Q1
        
        lower_bound = Q1 - 1.5 * IQR
        
        upper_bound = Q3 + 1.5 * IQR    
 
 #Determine outliers using upper and lower bounds
    
        outliers = treatment_regimens_data[(treatment_regimens_data['Tumor Volume (mm3)'] < lower_bound) | 
                                           (treatment_regimens_data['Tumor Volume (mm3)'] > upper_bound)]
        
        print(f"{treatment}'s potential outliers: {outliers['Tumor Volume (mm3)']}")
        
        print('---------------------------------------')          

![image](https://github.com/user-attachments/assets/85a811f8-9681-4d5a-b21d-a0a260752bf9)

# Generate a box plot that shows the distribution of the tumor volume for each treatment group.
    treatment_regimens = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']

#Create an empty list to fill with tumor volume data for each treatment group

    tumor_vol_list = []

#Loop through each treatment regimen and append the tumor volumes to the list

    for treatment in treatment_regimens:
    
        treatment_data = final_tumor_volume_df[final_tumor_volume_df['Drug Regimen'] == treatment]
        
        tumor_vol_list.append(treatment_data['Tumor Volume (mm3)'])

    outlier_marker = dict(markerfacecolor='red', markersize=10)

#Use plt.boxplot() to create the box plot

    plt.boxplot(tumor_vol_list,flierprops=outlier_marker)

#Set the x-axis labels to the treatment names

    plt.xticks(range(1, len(treatment_regimens) + 1), treatment_regimens)

#Set plot labels and title

    plt.xlabel("Drug Regimen")
    
    plt.ylabel("Tumor Volume (mm3)")
    
    plt.title("Tumor Volume Distribution Across Treatment Regimens")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/a7652e39-64dd-4675-9484-3a6b0508bc66)

# Line and Scatter Plots

# Generate a line plot of tumor volume vs. time point for a single mouse treated with Capomulin
    mouse_id = "y793"
    
    capomulin_data = combined_mouse_study_data[(combined_mouse_study_data['Drug Regimen'] == 'Capomulin') & (combined_mouse_study_data['Mouse ID'] == mouse_id)]
    
    timepoints = capomulin_data['Timepoint']
    
    tumor_volumes = capomulin_data['Tumor Volume (mm3)']
    
    plt.plot(timepoints, tumor_volumes)
    
    plt.xlabel("Timepoint (days)")
    
    plt.ylabel("Tumor Volume (mm3)")
    
    plt.title("Capomulin treatment of mouse y793")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/50841e17-44cf-4afe-a446-d5b9b45ac0a4)

# Generate a scatter plot of mouse weight vs. the average observed tumor volume for the entire Capomulin regimen

    capomulin_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Capomulin']
    
    capomulin_grouped = capomulin_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()
    
    plt.scatter(capomulin_grouped['Weight (g)'], capomulin_grouped['Tumor Volume (mm3)'])
    
    plt.xlabel("Weight (g)")
    
    plt.ylabel("Average Tumor Volume (mm3)")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/aac53f27-4e45-43e0-838a-3dc3efd7aa5a)

# Correlation and Regression

# Calculate the correlation coefficient and a linear regression model for mouse weight and average observed tumor volume for the entire Capomulin regimen
    capomulin_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Capomulin']
    
    capomulin_grouped = capomulin_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()

#To ensure the same calculation could be used for all Regimens in my analysis, I worked with a tutor Luke Manning, to assist in creating the correlation to specific Regimens

    correlation = capomulin_grouped['Weight (g)'].corr(capomulin_grouped['Tumor Volume (mm3)'])
    
    print(f"Correlation Coefficient between Mouse Weight and Average Tumor Volume: {correlation:.2f}")

#As I was unfamiliar with the foundational principles of calculating using Pearson R, my tutor Luke Manning also assisted with this part of my code, explaining in detail as to what is being used in each calculation so I could also use this to scale it to the other Regimens I intended to use in my analysis.

    corr=round(st.pearsonr(capomulin_grouped['Weight (g)'],capomulin_grouped['Tumor Volume (mm3)'])[0],2)
    
    model = st.linregress(capomulin_grouped['Weight (g)'],capomulin_grouped['Tumor Volume (mm3)'])
    
    y_values = capomulin_grouped['Weight (g)']*model[0]+model[1]
    
    plt.scatter(capomulin_grouped['Weight (g)'],capomulin_grouped['Tumor Volume (mm3)'])
    
    plt.plot(capomulin_grouped['Weight (g)'],y_values,color="red")
    
    plt.xlabel("Mouse Weight (g)")
    
    plt.ylabel("Average Tumor Volume (mm3)")
    
    plt.title("Capomulin Treatment")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/b7824731-df1f-4dd9-b028-6286296b4421)

# Further Analysis of 4 main Regimens to provide additional insights

    ramicane_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Ramicane']
    
    ramicane_grouped = ramicane_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()
    
    correlation = ramicane_grouped['Weight (g)'].corr(ramicane_grouped['Tumor Volume (mm3)'])
    
    print(f"Correlation Coefficient between Mouse Weight and Average Tumor Volume: {correlation:.2f}")
    
    corr=round(st.pearsonr(ramicane_grouped['Weight (g)'],ramicane_grouped['Tumor Volume (mm3)'])[0],2)
    
    model = st.linregress(ramicane_grouped['Weight (g)'],ramicane_grouped['Tumor Volume (mm3)'])
    
    y_values = ramicane_grouped['Weight (g)']*model[0]+model[1]
    
    plt.scatter(ramicane_grouped['Weight (g)'],ramicane_grouped['Tumor Volume (mm3)'])
    
    plt.plot(ramicane_grouped['Weight (g)'],y_values,color="red")
    
    plt.xlabel("Mouse Weight (g)")
    
    plt.ylabel("Average Tumor Volume (mm3)")
    
    plt.title("Ramicane Treatment")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/75b324c2-f11a-448b-8726-98223ca99a7b)

    infubinol_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Infubinol']
    
    infubinol_grouped = infubinol_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()
    
    correlation = infubinol_grouped['Weight (g)'].corr(infubinol_grouped['Tumor Volume (mm3)'])
    
    print(f"Correlation Coefficient between Mouse Weight and Average Tumor Volume: {correlation:.2f}")
    
    corr=round(st.pearsonr(infubinol_grouped['Weight (g)'],infubinol_grouped['Tumor Volume (mm3)'])[0],2)
    
    model = st.linregress(infubinol_grouped['Weight (g)'],infubinol_grouped['Tumor Volume (mm3)'])
    
    y_values = infubinol_grouped['Weight (g)']*model[0]+model[1]
    
    plt.scatter(infubinol_grouped['Weight (g)'],infubinol_grouped['Tumor Volume (mm3)'])
    
    plt.plot(infubinol_grouped['Weight (g)'],y_values,color="red")
    
    plt.xlabel("Mouse Weight (g)")
    
    plt.ylabel("Average Tumor Volume (mm3)")
    
    plt.title("Infubinol Treatment")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/b9931d31-3ca4-428b-900b-f0e01d66a45b)

    ceftamin_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Ceftamin']
    
    ceftamin_grouped = ceftamin_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()
    
    correlation = ceftamin_grouped['Weight (g)'].corr(ceftamin_grouped['Tumor Volume (mm3)'])
    
    print(f"Correlation Coefficient between Mouse Weight and Average Tumor Volume: {correlation:.2f}")
    
    corr=round(st.pearsonr(ceftamin_grouped['Weight (g)'],ceftamin_grouped['Tumor Volume (mm3)'])[0],2)
    
    model = st.linregress(ceftamin_grouped['Weight (g)'],ceftamin_grouped['Tumor Volume (mm3)'])
    
    y_values = ceftamin_grouped['Weight (g)']*model[0]+model[1]
    
    plt.scatter(ceftamin_grouped['Weight (g)'],ceftamin_grouped['Tumor Volume (mm3)'])
    
    plt.plot(ceftamin_grouped['Weight (g)'],y_values,color="red")
    
    plt.xlabel("Mouse Weight (g)")
    
    plt.ylabel("Average Tumor Volume (mm3)")
    
    plt.title("Ceftamin Treatment")
    
    plt.tight_layout()
    
    plt.show()

![image](https://github.com/user-attachments/assets/e84843d5-2f4c-485d-a4b3-661e9662904e)

# To support 

placebo_data = combined_mouse_study_data[combined_mouse_study_data['Drug Regimen'] == 'Placebo']

placebo_grouped = placebo_data.groupby(['Mouse ID'])[['Weight (g)', 'Tumor Volume (mm3)']].mean()

correlation = placebo_grouped['Weight (g)'].corr(placebo_grouped['Tumor Volume (mm3)'])

print(f"Correlation Coefficient between Mouse Weight and Average Tumor Volume: {correlation:.2f}")

corr=round(st.pearsonr(placebo_grouped['Weight (g)'],placebo_grouped['Tumor Volume (mm3)'])[0],2)

model = st.linregress(placebo_grouped['Weight (g)'],placebo_grouped['Tumor Volume (mm3)'])

y_values = placebo_grouped['Weight (g)']*model[0]+model[1]

plt.scatter(placebo_grouped['Weight (g)'],placebo_grouped['Tumor Volume (mm3)'])

plt.plot(placebo_grouped['Weight (g)'],y_values,color="red")

plt.xlabel("Mouse Weight (g)")

plt.ylabel("Average Tumor Volume (mm3)")

plt.title("Placebo Treatment")

plt.tight_layout()

plt.show()

![image](https://github.com/user-attachments/assets/aab19461-e393-40c1-9469-23d736641752)
