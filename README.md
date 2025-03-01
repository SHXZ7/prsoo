import pandas as pd

def calculate_demographic_data(print_data=True):
    # Read the dataset
    df = pd.read_csv("adult.data.csv")

    # Question 1: How many people of each race are represented?
    race_count = df["race"].value_counts()

    # Question 2: What is the average age of men?
    average_age_men = round(df[df["sex"] == "Male"]["age"].mean(), 1)

    # Question 3: What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = round((df["education"] == "Bachelors").mean() * 100, 1)

    # Question 4: Advanced education (Bachelors, Masters, Doctorate) and earning >50K
    advanced_education = df["education"].isin(["Bachelors", "Masters", "Doctorate"])
    higher_education_rich = round((df[advanced_education]["salary"] == ">50K").mean() * 100, 1)

    # Question 5: Percentage of people without advanced education making >50K
    lower_education_rich = round((df[~advanced_education]["salary"] == ">50K").mean() * 100, 1)

    # Question 6: What is the minimum number of hours a person works per week?
    min_work_hours = df["hours-per-week"].min()

    # Question 7: What percentage of the people who work the minimum number of hours per week have >50K salary?
    min_workers = df[df["hours-per-week"] == min_work_hours]
    rich_percentage = round((min_workers["salary"] == ">50K").mean() * 100, 1)

    # Question 8: Country with highest percentage of people earning >50K
    country_salary_group = df[df["salary"] == ">50K"]["native-country"].value_counts()
    country_total_group = df["native-country"].value_counts()
    country_percentage = (country_salary_group / country_total_group * 100).dropna()
    highest_earning_country = country_percentage.idxmax()
    highest_earning_country_percentage = round(country_percentage.max(), 1)

    # Question 9: Most popular occupation for those earning >50K in India
    india_high_income = df[(df["salary"] == ">50K") & (df["native-country"] == "India")]
    top_IN_occupation = india_high_income["occupation"].value_counts().idxmax()

    # Print results if needed
    if print_data:
        print("Race count:\n", race_count)
        print("Average age of men:", average_age_men)
        print("Percentage with Bachelors:", percentage_bachelors)
        print("Higher education earning >50K:", higher_education_rich)
        print("Lower education earning >50K:", lower_education_rich)
        print("Min work hours:", min_work_hours)
        print("Percentage of min workers earning >50K:", rich_percentage)
        print("Country with highest >50K percentage:", highest_earning_country, highest_earning_country_percentage)
        print("Top occupation in India for >50K earners:", top_IN_occupation)

    return {
        "race_count": race_count,
        "average_age_men": average_age_men,
        "percentage_bachelors": percentage_bachelors,
        "higher_education_rich": higher_education_rich,
        "lower_education_rich": lower_education_rich,
        "min_work_hours": min_work_hours,
        "rich_percentage": rich_percentage,
        "highest_earning_country": highest_earning_country,
        "highest_earning_country_percentage": highest_earning_country_percentage,
        "top_IN_occupation": top_IN_occupation,
    }
