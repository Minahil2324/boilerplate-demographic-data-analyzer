# Demographic Data Analyzer

This is the boilerplate for the Demographic Data Analyzer project. Instructions for building your project can be found at https://www.freecodecamp.org/learn/data-analysis-with-python/data-analysis-with-python-projects/demographic-data-analyzer
import pandas as pd

def calculate_demographic_data(print_data=True):
    # Load data
    df = pd.read_csv("adult.data.csv")

    # 1. How many of each race are represented?
    race_count = df['race'].value_counts()

    # 2. What is the average age of men?
    average_age_men = round(df[df['sex'] == 'Male']['age'].mean(), 1)

    # 3. What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = round(
        (df['education'] == 'Bachelors').sum() / len(df) * 100, 1)

    # 4. Percentage with and without advanced education earning >50K
    higher_education = df[df['education'].isin(
        ['Bachelors', 'Masters', 'Doctorate'])]
    lower_education = df[~df['education'].isin(
        ['Bachelors', 'Masters', 'Doctorate'])]

    higher_education_rich = round(
        (higher_education['salary'] == '>50K').sum() / len(higher_education) * 100, 1)
    lower_education_rich = round(
        (lower_education['salary'] == '>50K').sum() / len(lower_education) * 100, 1)

    # 5. Minimum number of hours a person works per week
    min_work_hours = df['hours-per-week'].min()

    # 6. What percentage of the people who work the minimum number of hours per week have a salary of >50K?
    min_workers = df[df['hours-per-week'] == min_work_hours]
    rich_percentage = round(
        (min_workers['salary'] == '>50K').sum() / len(min_workers) * 100, 1)

    # 7. Country with highest percentage of >50K earners
    country_rich = df[df['salary'] == '>50K']['native-country'].value_counts()
    country_total = df['native-country'].value_counts()
    rich_percentage_by_country = (country_rich / country_total * 100).dropna()
    highest_earning_country = rich_percentage_by_country.idxmax()
    highest_earning_country_percentage = round(
        rich_percentage_by_country.max(), 1)

    # 8. Most popular occupation for those who earn >50K in India
    india_occupations = df[(df['native-country'] == 'India')
                           & (df['salary'] == '>50K')]
    top_IN_occupation = india_occupations['occupation'].value_counts().idxmax()

    # Final results dictionary
    if print_data:
        print("Number of each race:\n", race_count)
        print("Average age of men:", average_age_men)
        print("Percentage with Bachelors degrees:", percentage_bachelors)
        print("Percentage with higher education that earn >50K:",
              higher_education_rich)
        print("Percentage without higher education that earn >50K:",
              lower_education_rich)
        print("Min work time:", min_work_hours, "hours/week")
        print("Percentage of rich among those who work fewest hours:",
              rich_percentage)
        print("Country with highest percentage of rich:", highest_earning_country)
        print("Highest percentage of rich people in country:",
              highest_earning_country_percentage)
        print("Top occupations in India:", top_IN_occupation)

    return {
        'race_count': race_count,
        'average_age_men': average_age_men,
        'percentage_bachelors': percentage_bachelors,
        'higher_education_rich': higher_education_rich,
        'lower_education_rich': lower_education_rich,
        'min_work_hours': min_work_hours,
        'rich_percentage': rich_percentage,
        'highest_earning_country': highest_earning_country,
        'highest_earning_country_percentage': highest_earning_country_percentage,
        'top_IN_occupation': top_IN_occupation
    }
python3 main.py
